<properties 
    pageTitle="Ομαλή ροή προσθήκης για το άνοιγμα αρχείου προέλευσης Framework πολυμέσων" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την προσθήκη Azure Media Services ομαλή ροή για το άνοιγμα Framework πολυμέσων προέλευσης Adobe." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Τον τρόπο χρήσης της Microsoft ομαλής ροής προσθήκης για το πλαίσιο Adobe Άνοιγμα αρχείου προέλευσης πολυμέσων

##<a name="overview"></a>Επισκόπηση


Η προσθήκη ομαλή ροή της Microsoft για άνοιγμα προέλευσης το πολυμέσων Framework 2.0 (ΕΕ για OSMF) επεκτείνει τις προεπιλεγμένες δυνατότητες του OSMF και προσθέτει ομαλή ροή Microsoft αναπαραγωγή περιεχομένου νέων και υπαρχόντων OSMF αναπαραγωγής. Η προσθήκη προσθέτει επίσης ομαλή ροή δυνατότητες αναπαραγωγής για την αναπαραγωγή πολυμέσων στροβοσκοπική (SMP).

SS για OSMF περιλαμβάνει δύο εκδόσεις του προσθήκης:

- Στατική Προσθήκη ομαλή ροή για OSMF (.swc)

- Δυναμική Προσθήκη ομαλή ροή για OSMF (.swf)

Αυτό το έγγραφο προϋποθέτει ότι το πρόγραμμα ανάγνωσης έχει γενικά γνώση του τρόπου εργασίας OSMF και OSMF προσθήκες. Για περισσότερες πληροφορίες σχετικά με το OSMF, ανατρέξτε στην τεκμηρίωση στην [επίσημη OSMF τοποθεσίας](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Ομαλή Προσθήκη ροής για OSMF 2.0

Η προσθήκη υποστηρίζει τη φόρτωση και αναπαραγωγή σε ζήτηση ομαλή ροή περιεχομένου με τις εξής δυνατότητες:

- Αναπαραγωγή ομαλή ροή σε ζήτηση (αναπαραγωγή, παύση, Seek, διακοπή)
- Ζωντανή ομαλή ροή αναπαραγωγή (Play)
- Live DVR συναρτήσεις (παύση, Seek, DVR αναπαραγωγής, μετάβαση ζωής)
- Υποστήριξη για κωδικοποιητές βίντεο - H.264
- Υποστήριξη για τους κωδικοποιητές ήχου - AAC
- Πολλές γλώσσα ήχου μετάβαση με ενσωματωμένα API OSMF
- Επιλογή ποιότητας αναπαραγωγής Max με ενσωματωμένα API OSMF
- Καλάθι κλειστές λεζάντες με Προσθήκη λεζάντων OSMF
- Adobe&reg; Flash&reg; προγράμματος αναπαραγωγής 11.4 ή νεότερη έκδοση.
- Αυτή η έκδοση υποστηρίζει μόνο OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Υποστηριζόμενες δυνατότητες και γνωστά θέματα

Για μια πλήρη λίστα των υποστηριζόμενες δυνατότητες, οι μη υποστηριζόμενες δυνατότητες και γνωστά θέματα, ανατρέξτε σε [αυτό το έγγραφο](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Κατά τη φόρτωση της προσθήκης
Είναι δυνατή η φόρτωση OSMF προσθήκες στατικά (κατά τη μεταγλώττιση) ή δυναμικά (κατά το χρόνο εκτέλεσης). Η προσθήκη ομαλή ροή για λήψη OSMF περιλαμβάνει τόσο δυναμικές και στατικές εκδόσεις.

- Στατική φόρτωσης: για να φορτώσετε στατικά, απαιτείται ένα αρχείο στατική βιβλιοθήκη (SWC). Στατική προσθήκες προστίθεται ως αναφορά στα έργα και συγχώνευση εντός του αρχείου τελικό αποτέλεσμα κατά τη μεταγλώττιση του.

- Δυναμική φόρτωση: για να φορτώσετε δυναμικά, απαιτείται ένα προ-μεταγλωττισμένη αρχείο (SWF). Δυναμική προσθήκες φορτώνονται στο χρόνο εκτέλεσης και δεν περιλαμβάνεται στο αποτέλεσμα του έργου. (Μεταγλωττισμένο εξόδου) Δυναμική προσθήκες μπορεί να μεταφερθεί χρησιμοποιώντας πρωτόκολλα HTTP και ΑΡΧΕΊΩΝ.

Για περισσότερες πληροφορίες σχετικά με τη φόρτωση στατικές και δυναμικές, ανατρέξτε στο θέμα επίσημη [OSMF Προσθήκη σελίδας](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS για τη φόρτωση στατική OSMF
Το τμήμα κώδικα παρακάτω δείχνει τον τρόπο για να φορτώσετε την προσθήκη SS για OSMF στατικά και αναπαραγωγή βίντεο βασική χρήση κλάσης OSMF MediaFactory. Πριν να συμπεριλάβετε το SS για OSMF κώδικα, βεβαιωθείτε ότι η αναφορά για το project περιλαμβάνει την προσθήκη στατικών "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc".

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS για δυναμική φόρτωση OSMF

Το τμήμα κώδικα παρακάτω δείχνει τον τρόπο για να φορτώσετε την προσθήκη SS για OSMF δυναμικά και αναπαραγάγετε μια βασική βίντεο χρησιμοποιώντας την κλάση OSMF MediaFactory. Πριν να συμπεριλάβετε το SS για OSMF κώδικα, αντιγραφή της προσθήκης δυναμικής "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" στο φάκελο έργου εάν θέλετε να φορτώσετε χρησιμοποιεί το πρωτόκολλο ΑΡΧΕΊΟΥ ή αντιγραφή κάτω από ένα διακομιστή web HTTP φόρτωση. Δεν χρειάζεται να συμπεριλάβετε "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" στις αναφορές έργου.

 
{πακέτου
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Στροβοσκοπική αναπαραγωγής πολυμέσων με την προσθήκη δυναμικού SS ODMF

Ομαλή ροή για προσθήκη δυναμικού OSMF είναι συμβατή με [Στροβοσκοπική αναπαραγωγής πολυμέσων (SMP)](http://osmf.org/strobe_mediaplayback.html). Μπορείτε να χρησιμοποιήσετε το SS για προσθήκη OSMF για να προσθέσετε ομαλή ροή περιεχομένου αναπαραγωγή SMP. Για να το κάνετε αυτό, αντιγράψτε "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" στην περιοχή σε διακομιστή web HTTP φόρτωση ακολουθώντας τα παρακάτω βήματα:

1.  Μεταβείτε στη [σελίδα εγκατάστασης στροβοσκοπική αναπαραγωγής πολυμέσων](http://osmf.org/dev/2.0gm/setup.html). 
2.  Ορίστε το src σε μια προέλευση ομαλή ροή, (π.χ., http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Κάντε τις αλλαγές επιθυμητή ρύθμιση παραμέτρων και κάντε κλικ στην επιλογή προεπισκόπηση και ενημέρωση.
 
    **Σημείωση** Ο διακομιστής περιεχομένου web χρειάζεται μια έγκυρη crossdomain.xml. 
4.  Αντιγράψτε και επικολλήστε τον κώδικα σε μια απλή σελίδα HTML χρησιμοποιώντας το πρόγραμμα επεξεργασίας κειμένου Αγαπημένα, όπως στο ακόλουθο παράδειγμα:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Χρήση προσθήκης ομαλή ροή OSMF τον κώδικα ενσωμάτωσης και αποθήκευση.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Αποθήκευση σελίδας HTML και δημοσίευση σε διακομιστή web. Μεταβείτε στη σελίδα στην δημοσιευμένη web χρησιμοποιώντας τις αγαπημένες Flash&reg; προγράμματος αναπαραγωγής με δυνατότητα προγράμματος περιήγησης στο Internet (Internet Explorer, Chrome, Firefox, κ.ο.κ).
7.  Απολαύστε ομαλή ροή περιεχομένου μέσα Adobe&reg; Flash&reg; προγράμματος αναπαραγωγής.

Για περισσότερες πληροφορίες σχετικά με γενικά OSMF ανάπτυξη, ανατρέξτε στο θέμα επίσημη [OSMF ανάπτυξης σελίδας](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Δείτε επίσης

[Microsoft προσαρμόσιμη ροής προσθήκης για ενημέρωση OSMF](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
