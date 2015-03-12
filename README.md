# django-socia-auth-demo
Facebook Login using Django

**Prerequisites**

Django 
python-social-auth https://github.com/omab/python-social-auth <br>
openid https://github.com/openid/python-openid <br>
jwt http://github.com/jpadilla/pyjwt <br>

First of all create a django project, then install python-social-auth,openid and jwt.


Now add following line to INSTALLED_APPS ,

```
'social.apps.django_app.default',
```

Add TEMPLATE_CONTEXT_PROCESSORS to settings.py file ,

```
TEMPLATE_CONTEXT_PROCESSORS = (
'django.contrib.auth.context_processors.auth',
'django.core.context_processors.debug',
'django.core.context_processors.i18n',
'django.core.context_processors.media',
'django.core.context_processors.static',
'django.core.context_processors.tz',
'django.contrib.messages.context_processors.messages',
'social.apps.django_app.context_processors.backends',
'social.apps.django_app.context_processors.login_redirect',
)
```

Add AUTHENTICATION_BACKENDS in settings.py,

```
AUTHENTICATION_BACKENDS = (
'social.backends.facebook.FacebookOAuth2',
'social.backends.google.GoogleOAuth2',
'social.backends.twitter.TwitterOAuth',
'django.contrib.auth.backends.ModelBackend',
)
```

Now add following lines to urls.py,

```
url('', include('social.apps.django_app.urls', namespace='social')),
url('', include('django.contrib.auth.urls', namespace='auth')),
```

Now crate a view as ,

```
from django.shortcuts import render_to_response
from django.template.context import RequestContext

def home(request):
	context = RequestContext(request, {'request': request,'user': request.user})
	return render_to_response('home.html',context_instance=context)
```

Update the urls.py with this line ,

```
url(r'^$', 'testapp.views.home', name='home'),
```

Now create template with this code called home.html,

```
<!DOCTYPE html PUBLIC “-//W3C//DTD HTML 4.01 Transitional//EN” “http://www.w3.org/TR/html4/loose.dtd”>
<html>

<head>
    <meta http-equiv=”Content-Type” content=”text/html; charset=ISO-8859-1″>
    <title>Insert title here</title>
</head>

<body>

    <div>
        <h1>Third-party authentication demo</h1>

        <p>
            <ul>
                {% if user and not user.is_anonymous %}
                <li>
                    <a>Hello {{ user.get_full_name|default:user.username }}!</a>
                </li>
                <li>
                    <a href="{% url 'auth:logout' %}?next={{ request.path }}">Logout</a>
                </li>
                {% else %}
                <li>
                    <a href="{% url 'social:begin' 'facebook' %}?next={{ request.path }}">Login with Facebook</a>
                </li>
                <li>
                    <a href="{% url 'social:begin' 'google-oauth2' %}?next={{ request.path }}">Login with Google</a>
                </li>
                <li>
                    <a href="{% url 'social:begin' 'twitter' %}?next={{ request.path }}">Login with Twitter</a>
                </li>
                {% endif %}
            </ul>
        </p>
    </div>

</body>

</html>
```

Now go to developers.facebook.com and create new application. In the settings of the newly-created application, click “Add Platform”. From the options provided, choose Web, and fill in the URL of the site (http://test1.com:8000 in our example).

Point your localhost:8000 to test1.com:8000

Copy the App ID and App Secret, and place them into settings.py file as,

```
SOCIAL_AUTH_FACEBOOK_KEY ='XXXXXXXXXXX'
SOCIAL_AUTH_FACEBOOK_SECRET ='XXXXXXXXXXXXXXXX'
```

Now run syncdb , and run your project.
	
