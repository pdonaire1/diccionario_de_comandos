Forgot password example FullStack:
==================================

A good option for make a complete password reset is folowing this [tutorial](http://ruddra.com/2015/09/18/implementation-of-forgot-reset-password-feature-in-django/).

Or You can user the next tutorial but instead of an external (Font) URL you can create your own Django Form.

Django Rest Service Forgot password:
====================================

We can use this [tutorial](https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/create_temporary_token.md)
for make an easier function.

**Install requirements**
```
pip install python-dateutil
```

**from [here:](https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/temporary_token.py)**
from .... import TemporaryToken 

**In our ```views.py```:**

```python
"""
    Created by: @pdonaire1 October 06, 2016
    Ing. Pablo Alejandro González Donaire
"""
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from django.conf import settings
from django.contrib.auth.models import User
from utils.temporary_token import TemporaryToken
from django.core.mail import send_mail
from django.template.loader import render_to_string
from rest_framework.authtoken.models import Token # Optional if you will use extra_token

class ResetPasswordViewSet(APIView):
    permission_classes = ()
    
    def send_forgot_password_email(self, user, email_token):
        template = render_to_string('./email/forgot_password.html',
            {
                'email_token': email_token,
                'username': user.username,
                'url': settings.URL_RECOVER_PASS})
        try:
            subject = "Recuperar usuario @ MyAPP"
            email_to = user.email
            from_email = settings.DEFAULT_FROM_EMAIL
            text_plain = "Hello,\n. Enter to retrieve your user.\Thanks."
            send_mail(subject, text_plain, from_email, [email_to], html_message=template)
        except: pass

    def get(self, request, format=None):
        username = request.GET.get("username", None)
        if username and User.objects.filter(username=username).exists():
            user = User.objects.get(username=username)
            # Let's send an email to recover password
            temporary_token = TemporaryToken(user)
            token = Token.objects.get_or_create(user=user)  # Optional
            token = token[0].key  # Optional for extra security
            email_token = temporary_token.hash_user_encode(
                hours_limit=24, extra_token=token)
            self.send_forgot_password_email(user, email_token)
            return Response([{
                "message": "An email has been send to your user",
                "success": True
            }])
        return Response([{
            "POST": "Reset password params: {email_token, new_password, username}",
            "GET": "Send email forgot password params: {username}",
        }])

    def post(self, request, format=None):
        """
            This function recibe: 
            email_token, new_password, username
        """
        email_token = request.data["email_token"]
        new_password = request.data["new_password"]
        username = request.data["username"]
        if User.objects.filter(username=username).exists():
            user = User.objects.get(username=username)
            temporary_token = TemporaryToken(user)
            # Optional this step is if was created an extra_token:
            token = Token.objects.get(user=user).key
            if temporary_token.hash_user_valid(email_token, extra_token=token):
                user.set_password(new_password)
                user.save()
                return Response(
                    [{"message": "Password changed", "success": True}], 
                    status=status.HTTP_201_CREATED)

        return Response([{"message": "Token or username error", "success": False}],
            status=status.HTTP_400_BAD_REQUEST)
```

**In urls.py:**
```python
from utils.views import ResetPasswordViewSet
urlpatterns = [
    .
    .
    .
    url(r'^api/reset-password/', ResetPasswordViewSet.as_view()),
]
```

**In our ```templates/email/forgot_password.html``` :**
```
<html>
    <body>
        For recover password enter to next 
        <a href="{{ url }}?email_token={{ email_token }}&username={{ username }}" 
          style="text-decoration: none; color: #1e88e5;">
          link (recover user).
        </a>
    </body>
</html>
```

**In ```project/settings.py```:**
```python
URL_RECOVER_PASS = 'front/url/forgot-password/'
EMAIL_USE_TLS = True
DEFAULT_FROM_EMAIL = 'test@gmail.com'
SERVER_EMAIL = 'test@gmail.com'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_HOST_USER = 'test@gmail.com'
EMAIL_HOST_PASSWORD = 'test123##'
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
```

Result:
=======

**In ```/api/forgot-password/```:** 

**Method GET:**

<img src="https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/forgot_password/step1.png" 
width="90%" height="90%" />

**Method GET: params: ```{"username"}```**, 
Example: ```/api/forgot-password/?username=pablo_donaire29@hotmail.com```

<img src="https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/forgot_password/step2.png" 
width="90%" height="90%" />

**My email:**

<img src="https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/forgot_password/step3.png" 
width="70%" height="70%" />

**Method POST: params: ```{"email_token", "new_password", "username"}```**

<img src="https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/forgot_password/step4.png" 
width="70%" height="70%" />

**And finally our response:**

<img src="https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/forgot_password/step5.png" 
width="70%" height="70%" />


