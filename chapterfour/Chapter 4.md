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
* @param receiver a receiver instance to cache its data.
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
- Legal comments.
    - Copyright and license notices, examples :
    [Jme3 copyright]
    ```java
    /*
     * Copyright (c) 2009-2021 jMonkeyEngine
     * All rights reserved.
     *
     * Redistribution and use in source and binary forms, with or without
     * modification, are permitted provided that the following conditions are
     * met:
     *
     * * Redistributions of source code must retain the above copyright
     *   notice, this list of conditions and the following disclaimer.
     *
     * * Redistributions in binary form must reproduce the above copyright
     *   notice, this list of conditions and the following disclaimer in the
     *   documentation and/or other materials provided with the distribution.
     *
     * * Neither the name of 'jMonkeyEngine' nor the names of its contributors
     *   may be used to endorse or promote products derived from this software
     *   without specific prior written permission.
     *
     * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
     * "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED
     * TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
     * PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR
     * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
     * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
     * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
     * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
     * LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
     * NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
     * SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
     */
    ```
    [Ccoffee Building tool]
    ```java
    #**
    #* Ccoffee Build tool, manual build, alpha-v1.
    #*
    #* @author pavl_g.
    #*#
    ```
    [Pi4J Framework]
    ```java
    /*
     * #%L
     * **********************************************************************
     * ORGANIZATION  :  Pi4J
     * PROJECT       :  Pi4J :: Java Library (Core)
     * FILENAME      :  AnalogInputListener.java
     *
     * This file is part of the Pi4J project. More information about
     * this project can be found here:  https://pi4j.com/
     * **********************************************************************
     * %%
     * Copyright (C) 2012 - 2021 Pi4J
     * %%
     * Licensed under the Apache License, Version 2.0 (the "License");
     * you may not use this file except in compliance with the License.
     * You may obtain a copy of the License at
     *
     *      http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     * #L%
     */
    ```
- Informative comments (better be in the form of javaDocs).
- Explanation of intent.
- Clarification.
- Warning of consequences.
- TODO comments.
- Amplification.
- JavaDocs in public APIs.
## Bad Comments :
