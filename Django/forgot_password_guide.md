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

class ResetPasswordViewSet(APIView):
    permission_classes = ()

    def get(self, request, format=None):
        return Response([{
            "message": "fields {email_token, new_password, username}"}])

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
                    [{"message": "password changed", "success": True}], 
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
