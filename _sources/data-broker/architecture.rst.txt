.. _architecture:

Data Broker architecture
========================

This page will show how the data-broker works and how it should work


Problem description (or why this component exists)
--------------------------------------------------

Data from various sources should be routed to their destinations properly
amidst dojot's components. This task should be performed in a orchestraded way,
with a single service (which may have multiple instances) being responsible for
it. The most proeminent problem is how to deal with tenant separation at Kafka
topic level, which is implemented by having a particular topic for each tenant
containing messages of the same nature.


Responsibilities
----------------

This component is responsible for:


- Keeping track of which Kafka topic is associated to `{subject, tenant}`;
- Sending messages and notifications to clients via socket.io;

.. note:: Clearly, there are some problems with responsibilities for this
   component. We could change the responsibility to be "being a general data
   broker", but this would lead to confusion about whether it should process *all*
   data transmitted in dojot, incluiding Kafka messages, socket.io requests,
   HTTP requests, requests sent to other services, such as postgres and Mongo DB,
   and so on. In other way, if we narrow down its responsibilities to "translate
   `{subject, tenant}` to Kafka topics and create a sub-component (or another
   component entirely) which will send to clients notifications and device
   messages via socket.io, everything should be way more comprehensive.



Architecture
------------







Resposibilities
---------------

These are the responsibilities

.. _initial_authentication:
.. uml::
   :caption: Initial authentication
   :align: center

   actor Client
   boundary Kong
   control Auth

   Client -> Kong: POST /auth \nBody={"admin", "p4ssw0rD"}
   activate Kong
   Kong -> Auth: POST /user \nBody={"admin", "p4ssw0rD"}
   Auth --> Kong: JWT="873927dab"
   Kong --> Client: JWT="873927dab"
   deactivate Kong
