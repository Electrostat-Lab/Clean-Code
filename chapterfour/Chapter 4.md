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
* Caches the receiver data for future use.
* @param receiver a receiver instance to cache its data.
*/
void Letter::initCache(Receiver& receiver) {
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
    - Quick translation of a meaning, Example :
    ```cxx
    const double* Meters::Chronometer::toDouble(const Clock* cpuClock) {
            // the cpuClock is the number of cpu ticks or cycles and by convention : CPU_F = cycles/seconds
            // so, seconds = cycles / CPU_F.
            // and to convert it to millisecond then multiply the result by 1000. 
            *time = (double) (*cpuClock / CPU_FREQUENCY) * 1000;
             delete cpuClock;
       return time;
    }
    ```
- Warning of consequences.
    - Some actions can have side effects, others are deprecated to call or aren't called manually, so we should warn our user using the API :
    ```java
        /**
         * @deprecated don't override, use {@link CompatHarness#onStartRenderer(LegacyApplication)}.
         */
        @Deprecated
        @Override
        protected void onStart() {
            super.onStart();
            try {
                startRenderer(EGLConfig.getEglSurfaceDelay());
            } catch (IllegalAccessException | InstantiationException e) {
                compatLogger.log(Level.WARNING, e.getMessage());
                onExceptionThrown(e);
            }
        }

        /**
         * @deprecated use {@link CompatHarness#onResumeRenderer(LegacyApplication)}.
         */
        @Deprecated
        @Override
        protected void onResume() {
            super.onResume();
            if(application != null){
                gainFocus();
                compatLogger.log(Level.INFO, "Game returns from the idle mode >>>>>>");
            }
        }

        /**
         * @deprecated never use, refer to {@link CompatHarness#onStopRenderer(LegacyApplication)}.
         */
        @Deprecated
        @Override
        protected void onStop() {
            super.onStop();
            if(isAppStateBound()){
                //release the static memory
                GameState.setApplication(null);
                GameState.setOglesContext(null);
            }
            delegateAppInstanceToState();
            //clean-up android views
            layoutHolder.removeAllViews();
            compatLogger.log(Level.INFO, "Game stops, delegating the app instance to the game states >>>>>>");
        }
    ```
    
    Here, you should never call these methods manually.
- TODO comments.
    - Somethings need to be done later or its trivial to do :
    ```java
    /**
     * An Android CompatHarness Migration test.
     * @author pavl_g.
     */
    public class TestCompatHarness extends CompatHarness {
        ....
        @Override
        protected void onExceptionThrown(Throwable throwable) {
            //TODO catch and deal with exceptions/errors here
        }

        @Override
        protected void onStartRenderer(LegacyApplication app) {
            //TODO do something when the renderer starts
        }

        @Override
        protected void onRendererCompletion(LegacyApplication app) {
            //TODO do something when the renderer completes
        }
        ....
    }
    ```
    
    An example code fragement from the new CompatHarness androidx migration of a jmonkeyengine game window shows some TODO comments to guide users what to do next
    when overriding these methods.
- Amplification.
    - Making sure of importance of something by illustrating it in depth or multiple times : 
    ```java
        /**
         * Initializes the gameStick view that would move the vehicle along the way
         * this is a brief way of calculations (for the illustrations) only :->
         * @apiNote <img src='image.png' width=300 height=200/>
         * @param stickBackground background drawable for the game Stick
         * @param stickImage the game Stick image
         * @param stickSize the stick size ( preferred : 100 , 150 , 200 ); in px
         *
         */
    ```
    The image here in javadocs although it's not a quite good way to write javadocs, but it still adds some sense about 
    how a vritual joystick would actually create an analog data for us.

- JavaDocs in public APIs.

## Bad Comments :
- Most comments fall under this category.
- Hurrying in writing a comment, beacuse the process requires it, is nothing but a hack, and the result is a crap :
    ```java
        /**
         * updatable via the device sensors
         * @param pulse angle of rotation in degrees converted to pulses
         */
        private void steerUsingDeviceOrientation(float pulse){
            /* doing interval(threshold) to start steering from ; ie ]-12,12[ are kept spare of steer listeners
            note : there are no mathematical formula fo these values , they are captured from physical testing & personal conclusions*/
            if( pulse<-12){
                if(gameStickListeners!=null){
                    gameStickListeners.steerRT(pulse / 10);
                }
            } else if(pulse>12){
                if(gameStickListeners!=null){
                    gameStickListeners.steerLT(pulse / 10);
                }
            }
        }
    ```
    
    Here the javaDocs adds no information for what this actually does, which is mapping the output pulse from a geomagenetic software sensor into left
    and right quadrants.
- Mumbling (talking to yourself) : when you write comments in a hurry or redundant comments.
- Redundant comments 
    - Comments that add no new information are just distractions at best.
    ```java
    //clean-up android views
    layoutHolder.removeAllViews();
    compatLogger.log(Level.INFO, "Game stops, delegating the app instance to the game states >>>>>>");
    ```  
    Here, the `//clean-up android views` is a redundant comment and misleading, all the users know the `removeAllViews()` detaches all widgets from a group
    widget but it doesn't really clean them up or destroy them !!
- Misleading comments
- Mandated comments :
- Journal comments.
- Noise comments.
- Scary noise.
- Never favours a comment over a function or method.
- Position markers
    - They create some distractions, but aren't bad all times
    ```java
       private DestructionPolicy destructionPolicy = DestructionPolicy.DESTROY_WHEN_FINISH;
        /*extra messages/data*/
        private String crashLog = "";
        private String glEsVersion = "";

        //******************Helper Classes************************

        /**
         * Used as a static memory to protect the game context from destruction by Activity#onDestroy().
         * For usages :
         *
         * @see DestructionPolicy
         * @see JmeSurfaceView#setDestructionPolicy(DestructionPolicy)
         */
        protected static final class GameState {
        .........
    ``` 
    `//******************Helper Classes************************` this is the position marker that marks the start of some embedded classes.
- Closing brace comments in loops and conditions
- Attributions and by-lines : sometimes are beneficial if the original author left the api without docs along away ago.
- Commented-out code : it's not necessary to comment out-dated code, if its working activate it, otherwise delete it.
- HTML Comments : too much html tags inside javadocs is distraction and noise at best.
    - Bad code :
    ```java
    /**
     * Base implementation of the interface {@link Tween} for the new animation system.
     * <p>
     * The Action class collects the animation actions from an array of {@link Tween}s into {@link Action#actions}, and it extracts the non-action {@link Tween}s
     * into a {@link BaseAction}. The net result is creating a holder that holds the Animation Actions and controls their properties including {@link Action#speed}, {@link Action#length},
     * {@link Action#mask} and {@link Action#forward}.
     * <br/>
     * <p>
     * Notes :
     * <li> The sequence of tweens is determined by {@link com.jme3.anim.tween.Tweens} utility class and {@link BaseAction} interpolates that sequence. </li>
     * <li> This implementation mimics the {@link com.jme3.anim.tween.AbstractTween}, but it delegates the interpolation method {@link Tween#interpolate(double)}
     * to the {@link BlendableAction} class.</li>
     *
     * <b>Created by Nehon.</b>
     *
     * @see BlendableAction
     * @see BaseAction
     */
    ```
    
    - Good code :
    ```java
    /**
     * Base implementation of the interface {@link Tween} for the new animation system.
     *
     * The Action class collects the animation actions from an array of {@link Tween}s into {@link Action#actions}, and it extracts the non-action {@link Tween}s
     * into a {@link BaseAction}. The net result is creating a holder that holds the Animation Actions and controls their properties including {@link Action#speed}, {@link Action#length},
     * {@link Action#mask} and {@link Action#forward}.
     * <br/>
     *
     * Notes :
     * <li> The sequence of tweens is determined by {@link com.jme3.anim.tween.Tweens} utility class and {@link BaseAction} interpolates that sequence. </li>
     * <li> This implementation mimics the {@link com.jme3.anim.tween.AbstractTween}, but it delegates the interpolation method {@link Tween#interpolate(double)}
     * to the {@link BlendableAction} class.</li>
     *
     * <b>Created by Nehon.</b>
     */
    ```
    
    As you can see we have shrinked the docs and removed some redundant html tags.
- Non-local information
- Too-much information
- Inobvious connection
- Function headers
- Javadocs in non-public code
 
