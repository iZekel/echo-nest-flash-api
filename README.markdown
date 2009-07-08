echo-nest-flash-api
===================

An ActionScript 3 interface for the the Echo Nest API.

http://github.com/also/echo-nest-flash-api

Getting the source
==================

You’ll need to clone the source from github, including the submodules:

    git clone git://github.com/also/echo-nest-flash-api.git
    cd echo-nest-flash-api
    git submodule update --init

Including in your project
=========================

Add both `echo-nest-flash-api/src` and `echo-nest-flash-api/lib/flash-net-utils` to your classpath. Your Flash, Flex, or mxmlc documentation tells you how to do this.

Using the API
=============

echo-nest-flash-api implements all of the [Echo Nest track API methods][1]. The methods provide data in a simple array format.

Analysis
--------

    var trackApi:TrackApi = new TrackApi();
    trackApi.apiKey = 'EJ1B4BFNYQOC56SGF';

    trackApi.getMode({id: 'music://id.echonest.com/~/TR/TRLFPPE11C3F10749F'}, {
      onResponse: function(mode:Array):void {
        trace('Mode: ' + mode[0] +', confidence: ' + mode[1]);  // Mode: 1, confidence: 1
      }
    });

Upload
------

    var fileReference:FileReference;

    private function chooseFile():void {
      fileReference = new FileReference();
      fileReference.addEventListener(Event.SELECT, function(e:Event):void {
        fileReference.addEventListener(Event.COMPLETE, function(e:Event):void {
          // file loaded, call uploadFile next
          trace('loaded ' + fileReference.name);
        });
        fileReference.load();
      });
      fileReference.browse();
    }

    private function uploadFile():void {
      trackApi.uploadFileData(fileReference.data, {
        onResponse: function(track:Object):void {
          trace('id: ' + track.id + ', md5: ' + track.md5);  // id: music://id.echonest.com/~/TR/TRMVA0211BC6E08329, md5: 2f45abacba9e9d2312afa63a8df10d23
        },
        onEchoNestError: function(error:EchoNestError):void {
          trace(error.code + ': ' + error.description);
        }
      });
    }

Due to security restrictions in Flash Player 10, both `chooseFile()` and `uploadFile()` must be called in response to user clicks.

License
=======

Copyright 2009 Ryan Berdeen. All rights reserved.  
Distributed under the terms of the MIT License.  
See accompanying file LICENSE.txt

 [1]: http://developer.echonest.com/pages/overview