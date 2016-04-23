---
title: Validation of attributes at different time points
date: 2016-03-07 16:56 UTC
tags: ruby, rails, validations, models
---

Say that we are creating an app to sell tickets. The idea is that we have a
certain amount of tickets to sell and we want to check that we are not
giving more tickets that the ones we have available.

One simple way of doing this is checking in the server side that the quantity
we are giving is equal or less than the remaining quantity.

Say that we have a Ticket and a Purchase models in our app like the following:

```ruby
# models/ticket.rb
class Ticket < ActiveRecord::Base
  has_many :purchases
  validate :tickets_available

  def remaining
    total - given
  end

  def given
    purchases.pluck(:quantity).reduce(:+) || 0
  end
end
```

```ruby
# models/purchase.rb
class  Purchase < ActiveRecord::Base
  belongs_to :ticket

  def tickets_available
    if ticket.remaining - quantity < 0
      errors.add(:quantity, "too many tickets given")
    end
  end
end
```

That seems to work fine until we have to deal with the situation of having to
_update_ a record with all the tickets given. For example this can happen as
administrators trying to fix an error in one of the purchased quantity.  We want
to fix a ticket record of 8 total tickets (and 8 given).  The record to update
is going to validate with the `tickets available` method from above. However,
the count `remaining - quantity < 0` will return _always_ a negative value, as
the remaining tickets is 0 (we had 8 tickets given, all the available ones).
Therefore the validation doesn't pass and we are not able to update the record
anymore.

Basically, the problem here is that we want to _replace_ the quantity of tickets
given, but our validation is taking into account the quantity of tickets
_already given_ in a previous purchase. We need to find a way of counting the
remaining tickets as if we had the tickets for this record still to give, not
already given. In other words, we have to simulate as if the customer would have
given  back the tickets purchased and we correct the purchase giving them the
right amount of tickets.

To do this, only on update, we need to get the quantity of tickets already given
(and stored in the record we want to fix), but we know that the variable
`quantity` contains the _new_ value of tickets given, not the old one. We need
the value of `quantity` in two different points in time, before and after the
update.

Fortunately Rails has a way of doing this by adding the suffix `_was` to the
name of the attribute in order to get its previous value. In this way we can
replace the method above for the following one:

```ruby
def tickets_available
  quantity_was ||= 0
  if remaining + quantity_was - quantity < 0
    errors.add(:quantity, "too many tickets given")
  end
end
```

Now for new records `quantity_was` will be 0 (as it is nil if the record is not
yet persisted) or it will contain the number of tickets to correct from the
record. This will work for new records or updates.

One additional detail about this validation is what happens in case we need to
update another attribute different from the quantity of the purchase record. In
this case the validation above will always pass (as the remaining value will be
at least 0). So this will work on updates of any attribute but will always fail
if the quantity of tickets to give is higher than the available ones, which is
another desirable outcome.


