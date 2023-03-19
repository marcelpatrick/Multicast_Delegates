# Multicast_Delegates

# Multicast_Delegates

# Definition of Multicast Delegates
  - Multicast Delegates can bound to more than one listner to trigger their callback functions and broadcast events to all of them at once.
  - If we try to bound single delegates to more then one callback function it will only trigger the function that was last bound to it. 

# 0- Create Classes
  - Create a Sender C++ class of AActor type that will send some information using the delegate
  - Create a Receiver C++ class of AActor type that will receive the information and use this to execute the callback function
  - Create a blueprint based on the Sender C++ class
  - Create a blueprint based on the Receiver C++ class
  - Place both blueprints in the world
  - In project settings, create a tag "Sender", in Sender BP include a new tag "Sender"

  ### DELEGATE: DECLARE (TEMPLATE) > DEFINE (BROADCAST) > CALLBACK: DECLARE > DEFINE > FIND > BIND > UNBIND

# 1- DELEGATE: DECLARE (TEMPLATE): 
