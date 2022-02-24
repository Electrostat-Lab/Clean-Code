# Comments
## Extra comments are failure :
- You should know that comments rather than the minimum mandatory html documentation in some cases are a sort of failure, 
where you fail to express the classes/functions from their names, so you try to hide that by explaining your mess.
- Explaining the code by using good names and putting your classes/functions in good contexts and also using small functions 
can help you minimize the extra comments for your application.
- Documenting a project should be a mandatory procedure but it shouldn't be too long or hard to understand.

## Explain yourself in code :
- It's even better to explain your code without worrying too much about generating a documentation, because the more the code is clean, 
the easier it would be to understand from a basic documenting line.
- Examples : 
```cxx
/**
* Letter lib.
* @author pavl_g.
*/
Letter::Letter(Sender& sender) {
    this->sender = sender;
}
/**
* Sends a letter to a receiver and saves the data as local 
* cache in this instance.
* @param receiver a receiver instance. 
* @param data a data instance to get the receiver data.
*/
void Letter::send(Receiver& receiver, Data& data) {
    if (data == NULL) {
        InvalidReceiverData("The data isn't valid !");
    }
    this->setUserName(data);
    this->setAccountType(data);
    this->setTextBody(data);
    this->setTextSubject(data);
    this->sender.send(&data);
}
void Letter::setAccountType(Data& data) {
  ....
}

void Letter::setTextBody(Data& data) {
  ....
}
...
```
A better apporach is to separate the `cache` from the `send` methods and migrate the data caching to the `Receiver class` as an `embedded struct`
```cxx
/**
* Letter lib.
* @author pavl_g.
*/
Letter::Letter(Sender& sender) {
    this->sender = sender;
}
/**
* Caches the receiver data inside this instance for future use.
* @para, receiver a receiver instance to cache its data.
*/
bool Letter::initCache(Receiver& receiver) {
    if (data == NULL) {
        InvalidReceiverData("The data isn't valid !");
    }
    this->setUserName(receiver.getData()->getUserName());
    this->setAccountType(receiver.getData()->getAccountType());
    this->setTextBody(receiver.getData()->getTextBody());
    this->setTextSubject(receiver.getData()->getTextSubject());
}

/**
* Sends a letter to a receiver.
* cache in this instance.
* @param receiver a receiver instance. 
*/
void Letter::send(Receiver& receiver) {
    this->sender.send(recevier.getData());
}
/* Data cachers methods */
void Letter::setAccountType(const char* accountType) {
  ....
}

void Letter::setTextBody(const char* textBody) {
  ....
}
...
```
## Good comments :
## Bad Comments :
