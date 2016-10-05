Forgot password example fullstack:
==================================

A good option for make a complete password reset is folowing this [tutorial](http://ruddra.com/2015/09/18/implementation-of-forgot-reset-password-feature-in-django/).

Django Rest Service Forgot password:
====================================

We can use this [tutorial](https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/create_temporary_token.md)
for make an easier function.

**from [here:](https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/temporary_token.py)**
from .... import TemporaryToken 

**in our ```views.py```: **

```python
from rest_framework.views import APIView
from django.contrib.auth.models import User
from utils.temporary_token import TemporaryToken
from django.core.mail import send_mail
from django.template.loader import render_to_string

class ResetPasswordViewSet(APIView):
    permission_classes = ()
    def send_forgot_password_email(self, user, email_token):
        template = render_to_string('./email/forgot_password.html',
                                        {'email_token': email_token,
                                        'url': settings.URL_RECOVER_PASS})
        try:
            subject = "Recuperar usuario @ Mall4G"
            email_to = user.email
            from_email = settings.DEFAULT_FROM_EMAIL
            text_plain = "Hola,\n. Ingrese para recuperar su usuario.\nGracias."
            send_mail(subject, text_plain, from_email, [email_to], html_message=template)
        except: pass

    def get(self, request, format=None):
        username = request.GET.get("username", None)
        if username and User.objects.filter(username=username).exists():
            user = User.objects.get(username=username)
            # Let's send an email to recover password
            temporary_token = TemporaryToken(user)
            email_token = temporary_token.hash_user_encode(hours_limit=24)
            self.send_forgot_password_email(user, email_token)
            return Response([{
                "message": "An email has been send to your user",
                "success": True,
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
            if temporary_token.hash_user_valid(email_token):
                user.set_password(new_password)
                user.save()
                return Response(
                    [{"message": "password cambiada", "success": True}], 
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
        <a href="{{url}}?email_token={{email_token}}" style="text-decoration: none; color: #1e88e5;">
          link (recover user).
        </a>
    </body>
</html>
```

**In ```project/settings.py```:**
```
URL_RECOVER_PASS = 'front/url/forgot-password/'
```




