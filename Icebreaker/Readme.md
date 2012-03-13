REMOVE APP FROM PRIVACY SETTINGS

STEP 1:

SUMMARY: Boilerplate HTML

&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;Tel Aviv Hack&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
Hello Tel Aviv!
&lt;/body&gt;
&lt;/html&gt;


--------------------------------------------------------------

STEP 1a:

SUMMARY: Text is zoomed, set viewport

&lt;meta name=&quot;viewport&quot; content=&quot;initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no&quot; /&gt;


--------------------------------------------------------------


STEP 2:

SUMMARY: Set up app in dev settings (https://developers.facebook.com/apps), fill in app domain to your host URL.

Note the app id and replace this with the &lt;APP_ID&gt; below:

  &lt;div id=&quot;fb-root&quot;&gt;&lt;/div&gt;

  &lt;script&gt;
  (function() {
    var e = document.createElement(&#39;script&#39;);
	e.async = true;
    e.src = document.location.protocol + &#39;//connect.facebook.net/en_US/all.js&#39;;
    document.getElementById(&#39;fb-root&#39;).appendChild(e);
    }());

  window.fbAsyncInit = function() {
    FB.init({ appId: &#39;&lt;APP_ID&gt;&#39;,
    status: true,
    cookie: true,
    xfbml: true,
    oauth: true});

    FB.Event.subscribe(&#39;auth.statusChange&#39;, handleStatusChange);
  };

  function handleStatusChange(response) {
    console.log(response);
  }

  &lt;/script&gt;


--------------------------------------------------------------
Login:

STEP 2a:

Add Login link:

&lt;div id=&quot;user-info&quot;&gt;Hello Tel Aviv!&lt;br&gt;
  &lt;a href=&quot;#&quot; onclick=&quot;login();&quot;&gt;Login&lt;/a&gt;&lt;br&gt;
&lt;/div&gt;

function login() {
  FB.login(function(response) { }, {scope:&#39;user_likes&#39;});
}

--------------------------------------------------------------


STEP 3:

Update User Info and get his name and profile pic:


  function handleStatusChange(response) {
    document.body.className = response.authResponse ? &#39;connected&#39; : &#39;not_connected&#39;;

    if (response.authResponse) {
      console.log(response);
      updateUserInfo(response);
    }
  }

  function updateUserInfo(response) {
    FB.api(&#39;/me&amp;fields=likes,id,name&#39;, function(response) {
      document.getElementById(&#39;user-info&#39;).innerHTML = &#39;&lt;img src=&quot;https://graph.facebook.com/&#39; + response.id + &#39;/picture&quot;&gt;&#39; + response.name;
    });
  }




--------------------------------------------------------------


STEP 4:

Get Common Likes:

Replace the following in the api.php -&gt; &lt;APP_ID&gt; and &lt;APP_SECRET&gt; with your app id and app secret.

You will also need to setup a mysql database and initiate in the db.php - mysql_connect(&#39;&lt;SERVER_NAME&gt;&#39;, &#39;&lt;USER_NAME&gt;&#39;, &#39;&lt;PASSWORD&gt;&#39;);

  ADD TO HEAD:
  &lt;script src=&quot;jquery-1.5.1.min.js&quot;&gt;&lt;/script&gt;


  ADD TO RESPONSE.AUTHRESPONSE:

  getCommonLikes(response);


  function getCommonLikes(response) {
      var output = &#39;&#39;;

    navigator.geolocation.getCurrentPosition(function(location) {
      $.ajax({url: &#39;api.php&#39;, type: &quot;POST&quot;, dataType: &#39;json&#39;, data: { method: &quot;getCommonLikes&quot;, location: location.coords }, success: function(data) {
        output = &#39;&#39;;

        console.log(data);

        for (var i = 0; i &lt; data.likes.length; i++) {
          output += &#39;&lt;h3&gt;&#39; + &#39;&lt;img src=&quot;http://graph.facebook.com/&#39; + data.likes[i].like_id + &#39;/picture&quot;&gt; &#39; + data.likes[i].like_name + &#39;&lt;h3&gt;&#39;;

          if (data.likes[i].uids) {
            output += &#39;&lt;h4&gt;Shared with&lt;/h4&gt;&#39;;
            for (var n = 0; n &lt; data.likes[i].uids.length; n++) {
              output += &#39;&lt;img src=&quot;http://graph.facebook.com/&#39; + data.likes[i].uids[n].id + &#39;/picture&quot;&gt; &#39; + data.likes[i].uids[n].name;
            }

            output += &#39;&lt;br&gt;&lt;br&gt;&#39;;
          }
        }

        $(&quot;body&quot;).append(output);
        console.log(output);
      }});
    },
    function(error) {
      alert(&#39;You need to allow location, first.  Please refresh.&#39;);
    });
  }


--------------------------------------------------------------


STEP 5:

Publish and Request Dialogs:

  function publishStory() {
    FB.ui({
      method: &#39;feed&#39;,
      name: &#39;I\&#39;m building a social mobile web app!&#39;,
      caption: &#39;This web app is going to be awesome.&#39;,
      description: &#39;Check out Facebook\&#39;s developer site to start building.&#39;,
      link: &#39;http://myfbse.com/tlv&#39;,
      picture: &#39;http://www.facebookmobileweb.com/hackbook/img/facebook_icon_large.png&#39;
    },
    function(response) {
      console.log(&#39;publishStory response: &#39;, response);
    });
    return false;
  }

  function sendRequest() {
    FB.ui({
      method: &#39;apprequests&#39;,
      message: &#39;Check out this awesome app!&#39;
    },
    function(response) {
      console.log(&#39;sendRequest response: &#39;, response);
    });
  }





  &lt;a href=&quot;#&quot; onclick=&quot;publishStory();&quot;&gt;Publish feed story&lt;/a&gt;&lt;br&gt;

  &lt;a href=&quot;#&quot; onclick=&quot;sendRequest();&quot;&gt;Send request&lt;/a&gt;&lt;br&gt;


Send Request, show request on m.facebook.com. Note that it doesn&#39;t show up.

Fill the mobile web URl, resend and notice that it shows up this time.


----------------

Step 6:


Authenticated referral.


----------------

Step 7:

Like and Comment Plugin

&lt;fb:like width=&quot;250&quot;&gt;&lt;/fb:like&gt;
&lt;fb:comments href=&quot;http://www.tunedon.com/testing/icebreaker/&quot; num_posts=&quot;4&quot; width=&quot;300&quot;&gt;&lt;/fb:comments&gt;