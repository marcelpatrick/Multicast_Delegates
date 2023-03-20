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
public:	
	void Send();
	MyMulticastDelegate MyDelegate;

};
``` 

# 2- DELEGATE: DEFINE (BROADCAST)
  - in the Sender implementation file, Implement the Send() function and in it use the delegate object to broadcast the content to the callback receiver function and any other function that subscribes to this broadcast.
  - Call Send() on BeginPlay()

```cpp
void ASender::BeginPlay()
{
	Super::BeginPlay();

	Send();
}

void ASender::Send()
{
	MyDelegate.Broadcast("This is the message from the send function in the Delegate");
}
```

# 3- CALLBACK: DECLARE
  - In the Receiver header file, declare an instance variable of the sender class and forward declare it (also include the reference for this class)
  - Declare a delegate handle to manage the callback events and delegate invocation list
  - Declare a Receive() function to receive the FString param broadcasted by the Delegate
  - Declare a Find() function to find the Sender actors in the world
  - Declare a Bind() function to subscrive the Receibe() callback function in the Receiver to the Send() delegate function in the Sender

```cpp
public:	
	UFUNCTION()
	void Receive(FString Message);
	
	void Find();	
	void Bind();

private:
	class ASender* MySender;
	FDelegateHandle MyDelegateHandle; 
```

# 4- CALLBACK: DEFINE
  - In the receiver implementation file, define Find() to find all actors with "Sender" tag and append them to an array of actors
  - Fetch the first element in the array, cast it to a Sender type and save it into the sender class instance variable
  - Define the Bind() function. Use the sender object to access its delegate object and bind it to the callback function Receive() using AddUObject and subscribing it to the delegate's invocation list
  - Define the Receive() function to receive the FString param and print it  
  - Call Find() and Bind() from BeginPlay()

```cpp
void AReceiver::BeginPlay()
{
	Super::BeginPlay();

	Find();

	Bind();
	
}

void AReceiver::Find()
{
	//Find all Sender actors in the world and save them in the actor array
	TArray<AActor*> TaggedActors;
	UGameplayStatics::GetAllActorsWithTag(GetWorld(), "Sender", TaggedActors);

	if (TaggedActors.Num() > 0)
	{
		//If you found them, save the first one in MySender var
		MySender = Cast<ASender>(TaggedActors[0]);
	}
	else
	{
		UE_LOG(LogTemp, Warning, TEXT("No actor found!!!"));

		return;
	}
}

void AReceiver::Bind()
{
	//Receiver class subscribes the Receive() function to the Delegate function in the Sender class
	MyDelegateHandle = MySender->MyDelegate.AddUObject(this, &AReceiver::Receive);
}

void AReceiver::Receive(FString Message)
{
	UE_LOG(LogTemp, Warning, TEXT("The received message is: %s"), *Message);
}
```








































