# deadset

A python class that provides item level expiration functionality for redis.
The expiration collector is NOT run automagically, you need to call it. See
the example below.

# Example usage

```
import random
from time import sleep
from pprint import pprint
from deadset import DeadSet

q5m = DeadSet(keyname="ITEMS_DIE_HERE",
              default_expire=datetime.timedelta(minutes=5))

# this item we are adding _MAY_ stop existing in 5m
q5m.add_expiring_item(itemId=str(random.random()))

print("UPDATES WILL BE PRINTED EVERY 60 SECONDS")

while True:
   ret = q5m.expire_items()
   zs = q5m.zscan()
   print("Number of items deleted: {}".format(ret))
   pprint(zs)
   if ret > 0:
      break
   sleep(60)
```
# Links to inspiration sources:

[TTL for a Set Member](https://stackoverflow.com/questions/17060672/ttl-for-a-set-member)
[How to expire List Items inRedis](https://quickleft.com/blog/how-to-create-and-expire-list-items-in-redis/)
