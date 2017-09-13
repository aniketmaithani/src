.. title: Sending BULK DATA using REQUEST LIBRARY
.. slug: sending-json-array-using-request-library
.. date: 2017-09-13 12:17:09 UTC
.. tags: DRF, Python, Requests
.. category: Tech
.. link:
.. description:
.. type: text

Few days back I was fiddling with my Raspberry Pi and found out that when my program sends a POST request it sends around 100 requests on an average. I later found out that for every record that gets registered on my device [basically some data I am capturing through sensors] it sends POST request for every data recorded! That's insane right!

For those who are thinking why? Well, let me tell you :

- Suppose there are 'n' records which are to be sent. Let's say that number in 10000
- Now every POST request takes about 0.5 seconds to complete
- Total Time Elapsed : 0.5 * 10000 = 5000 seconds!!

Now you get the gravity of the situation.

And this POST request hit my endpoint `/api/datacollection/` along with the data obviously. Now my tech-stack is Python/Django based and I am using THE AWESOME DRF for creating API's. The only work around I thought of this problem was if by any case I would be able to send data in bulk without fiddling the current `viewset` that I have written.

Solution -
1. Override the .create() method
2. Look for some third party library.

And voila! I went for the second solution and which was quite easy in nature. Introducing : (Django Rest Framework) [https://github.com/miki725/django-rest-framework-bulk] .

For this I just have to change my viewset to :

.. code:: python


  from rest_framework_bulk import ( BulkListSerializer, BulkSerializerMixin, ListBulkCreateUpdateDestroyAPIView)

     class MyDataViewset(ListBulkCreateUpdateDestroyAPIView):


And without touching other piece of code it worked just fine :D

And subsequently I was able to send around 5000 records in around 4.8912 seconds! Haha

For those who are wondering my json structure was :

.. code:: python

  data = [ {'uid': '87fbe152-4803-4953-9d76-266078dad892',
  'count': 4,
  'count_sensor_flips': 4,
  'device_id': 'f72448db-f156-4d50-afc6-ed294509c041',},
 {'uid': '87fbe152-4803-4953-9d76-266078dad892',
  'count': 4,
  'count_sensor_flips': 4,
  'devce_id': 'f72448db-f156-4d50-afc6-ed294509c041',},
 {'uid': '87fbe152-4803-4953-9d76-266078dad892',
  'count': 4,
  'count_sensor_flips': 4,
  'devce_id': 'f72448db-f156-4d50-afc6-ed294509c041',}]



Finally Goal Achieved.

Till the next time....ADIOS!
