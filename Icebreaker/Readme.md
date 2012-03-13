REMOVE APP FROM PRIVACY SETTINGS

STEP 1:

SUMMARY: Boilerplate HTML

<html>
<head>
  <title>Tel Aviv Hack</title>
</head>
<body>
Hello Tel Aviv!
</body>
</html>


--------------------------------------------------------------

STEP 1a:

SUMMARY: Text is zoomed, set viewport

<meta name="viewport" content="initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />


--------------------------------------------------------------


STEP 2:

SUMMARY: Set up app in dev settings (https://developers.facebook.com/apps), fill in app domain to your host URL.

Note the app id and replace this with the <APP_ID> below:

  <div id="fb-root"></div>

  <script>
  (function() {
    var e = document.createElement('script');
	e.async = true;
    e.src = document.location.protocol + '//connect.facebook.net/en_US/all.js';
    document.getElementById('fb-root').appendChild(e);
    }());

  window.fbAsyncInit = function() {
    FB.init({ appId: '<APP_ID>',
    status: true,
    cookie: true,
    xfbml: true,
    oauth: true});

    FB.Event.subscribe('auth.statusChange', handleStatusChange);
  };

  function handleStatusChange(response) {
    console.log(response);
  }

  </script>


--------------------------------------------------------------
Login:

STEP 2a:

Add Login link:

<div id="user-info">Hello Tel Aviv!<br>
  <a href="#" onclick="login();">Login</a><br>
</div>

function login() {
  FB.login(function(response) { }, {scope:'user_likes'});
}

--------------------------------------------------------------


STEP 3:

Update User Info and get his name and profile pic:


  function handleStatusChange(response) {
    document.body.className = response.authResponse ? 'connected' : 'not_connected';

    if (response.authResponse) {
      console.log(response);
      updateUserInfo(response);
    }
  }

  function updateUserInfo(response) {
    FB.api('/me&fields=likes,id,name', function(response) {
      document.getElementById('user-info').innerHTML = '<img src="https://graph.facebook.com/' + response.id + '/picture">' + response.name;
    });
  }




--------------------------------------------------------------


STEP 4:

Get Common Likes:

Replace the following in the api.php -> <APP_ID> and <APP_SECRET> with your app id and app secret.

You will also need to setup a mysql database and initiate in the db.php - mysql_connect('<SERVER_NAME>', '<USER_NAME>', '<PASSWORD>');

  ADD TO HEAD:
  <script src="jquery-1.5.1.min.js"></script>


  ADD TO RESPONSE.AUTHRESPONSE:

  getCommonLikes(response);


  function getCommonLikes(response) {
      var output = '';

    navigator.geolocation.getCurrentPosition(function(location) {
      $.ajax({url: 'api.php', type: "POST", dataType: 'json', data: { method: "getCommonLikes", location: location.coords }, success: function(data) {
        output = '';

        console.log(data);

        for (var i = 0; i < data.likes.length; i++) {
          output += '<h3>' + '<img src="http://graph.facebook.com/' + data.likes[i].like_id + '/picture"> ' + data.likes[i].like_name + '<h3>';

          if (data.likes[i].uids) {
            output += '<h4>Shared with</h4>';
            for (var n = 0; n < data.likes[i].uids.length; n++) {
              output += '<img src="http://graph.facebook.com/' + data.likes[i].uids[n].id + '/picture"> ' + data.likes[i].uids[n].name;
            }

            output += '<br><br>';
          }
        }

        $("body").append(output);
        console.log(output);
      }});
    },
    function(error) {
      alert('You need to allow location, first.  Please refresh.');
    });
  }


--------------------------------------------------------------


STEP 5:

Publish and Request Dialogs:

  function publishStory() {
    FB.ui({
      method: 'feed',
      name: 'I\'m building a social mobile web app!',
      caption: 'This web app is going to be awesome.',
      description: 'Check out Facebook\'s developer site to start building.',
      link: 'http://myfbse.com/tlv',
      picture: 'http://www.facebookmobileweb.com/hackbook/img/facebook_icon_large.png'
    },
    function(response) {
      console.log('publishStory response: ', response);
    });
    return false;
  }

  function sendRequest() {
    FB.ui({
      method: 'apprequests',
      message: 'Check out this awesome app!'
    },
    function(response) {
      console.log('sendRequest response: ', response);
    });
  }





  <a href="#" onclick="publishStory();">Publish feed story</a><br>

  <a href="#" onclick="sendRequest();">Send request</a><br>


Send Request, show request on m.facebook.com. Note that it doesn't show up.

Fill the mobile web URl, resend and notice that it shows up this time.


----------------

Step 6:


Authenticated referral.


----------------

Step 7:

Like and Comment Plugin

<fb:like width="250"></fb:like>
<fb:comments href="http://www.tunedon.com/testing/icebreaker/" num_posts="4" width="300"></fb:comments>