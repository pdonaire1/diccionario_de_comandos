Change password in Django-Rest-Services
=======================================

**In urls.py**
```
from utils.views import ChangePasswordViewSet
urlpatterns = [
    ...
    url(r'^api/v1/change-password/', ChangePasswordViewSet.as_view()),
]
```

**In `utils/views.py`**
```
from django.contrib.auth.models import User
from rest_framework.response import Response
from rest_framework.views import APIView

class ChangePasswordViewSet(APIView):
    permission_classes = ()
    
    def post(self, request, format=None):
        """
            This function recibe: 
            old_password, new_password, username
        """
        old_password = request.data["old_password"]
        new_password = request.data["new_password"]
        username = request.data["username"]
        user = User.objects.filter(username=username)
        if user.check_password(old_password):    
            user.set_password(new_password)
            user.save()
            return Response(
                [{"message": "Clave cambiada satisfactoriamente", "success": True}], 
                status=status.HTTP_201_CREATED)

        return Response([{"message": "Datos invalidos", "success": False}],
            status=status.HTTP_400_BAD_REQUEST)

```
