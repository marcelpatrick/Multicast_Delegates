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
  
  ### SENDER > DELEGATE BROADCAST > STRING > RECEIVER > RECEIVER BIND > RECEIVE > STRING > PRINT > UNBIND

# 1- DELEGATE: DECLARE (TEMPLATE): 
  - In the Sender class headerfile, use the delegate declaration template function of type multicast oneparam, choose a name and variable type it will broadcast
  - Declare a delegate object
  - Declare a sender function

```cpp
DECLARE_MULTICAST_DELEGATE_OneParam(MyMulticastDelegate, FString); 

UCLASS()
class MULTICAST_DELEGATES_API ASender : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	ASender();

	void Send();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	MyMulticastDelegate MyDelegate;

};
``` 

# 2- DELEGATE: DEFINE (BROADCAST)
  - in the Sender implementation file, Implement the Send() function and in it use the delegate object to broadcast the content to the callback receiver function

```cpp
void ASender::Send()
{
	MyDelegate.Broadcast("This is the message from the send function in the Delegate");
}
```

# 3- CALLBACK: DECLARE
  - In the Receiver header file, declare an instance of the sender class 
  - Declare a delegate handle to manage the callback events and delegate invocation list
  - Declare a Receive() function to receive the FString param broadcasted by the Delegate













































