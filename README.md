# Video_switcher
Video switcher prototype that enables time-based switching between video clips using Popcorn.js

The video switcher allows to dynamically switch between two or three video clips, either by holding or pressing a key on the keyboard.

##So far, his project contains 2 prototypes:

1. 2videos_keypress: Hold any key on the keyboard to switch to a second video clip and release key to display clip 1 again
2. 3videos_keyswitch: Switch between 3 video clips in real-time by pressing any key on the keyboard

The prototypes can be tested at:
http://melmoeller.com/Video_switcher/2videos_keypress.html
http://melmoeller.com/Video_switcher/3videos_keyswitch.html
PLEASE NOTE THAT THEY ONLY WORK ON CHROME AT THE MOMENT!

##Setup
All videos are being pre-loaded. Preload is set to 'auto'. This means that the data begins loading as soon as possible.
In prototype two videos 2 and 3 are hidden and muted on page load and the current video playing is video 1.
```
//Preload videos
    pop1.preload("auto");
    pop2.preload("auto");
    pop3.preload("auto");

    //Hide the other videos
    $("#video2").hide();
    $("#video3").hide();

    //Mute them
    pop2.mute();
    pop3.mute();

    currentVideo = 1;

    //Start the videos
    pop1.play();
    pop2.play();
    pop3.play();
```
##Key Events and Switching
Prototype 1: On keyup or keydown events, the app figures out what time we are at in the video with 'CurrentTime' and
by parsing in 'time', the second video plays exactly from the same point in time where we are at in the video.
Then the other (preloaded) video is being played using a play event. Using simple JQuery hide and show events, hides and shows the videos when switching between clips.

```
$(document).keyup(
      function(event)
      {
        var time = pop2.currentTime();
        pop2.pause();
        $("#video2").hide();
        $("#video1").show();
        pop1.play(time);
        allowPress = true;
      }
    );
```
Prototype 2: The mute and unmute media methods mute the videos that are not selected/ displayed.
```
    //On Key press events
    $(document).keypress(
      function(event)
      {
        if(currentVideo == 1)
        {
          pop1.mute();
          pop2.unmute();
          pop3.mute();

          $("#video1").hide();
          $("#video2").show();
          $("#video3").hide();

          currentVideo = 2;
        }
        else if(currentVideo == 2)
        {
          pop1.mute();
          pop2.mute();
          pop3.unmute();

          $("#video1").hide();
          $("#video2").hide();
          $("#video3").show();

          currentVideo = 3;
        }
        else
        {
          pop1.unmute();
          pop2.mute();
          pop3.mute();

          $("#video1").show();
          $("#video2").hide();
          $("#video3").hide();

          currentVideo = 1;
        }
      }
    );
  });
```
Events for each video define when all video clips are being played or paused.
```
 var videoElement1 = document.getElementById("video1");
    videoElement1.onpause = function() {
      var time = pop1.currentTime();
      pop2.currentTime(time);
      pop3.currentTime(time);
      pop2.pause();
      pop3.pause();
    };
    videoElement1.onplay = function() {
      pop2.play();
      pop3.play();
    };
```

Same as in prototype one, Jquery 'hide' and 'show' are used to display and hide clips.

Both prototypes feature a responsive video player and use test footage from a short film I produced a couple of years ago.




