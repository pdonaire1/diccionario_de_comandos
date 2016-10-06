TemporaryToken by User (customizable):
======================================
[Download](https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/temporary_token.py) 
The file ```temporary_token.py``` and right after that follow the steps showed below.

To create tokens temporary or unlimited by user you can use this class or you can modify it.

Created by **@pdonaire1** October 5, 2016.

First, import the class and use it, like this:
==============================================
You have to make a file with the Class content in second step.
Example: create a file ```temporary_token.py``` and import the class in your python file ```from temporary_token import TemporaryToken```

```python
from .... import TemporaryToken
from django.contrib.auth.models import User
user = User.objects.all().last()
temporary_token = TemporaryToken(user)
token_encode = temporary_token.hash_user_encode()
# or you can create a token with a limit of one day:
token_limited = temporary_token.hash_user_encode(hours_limit=24)
temporary_token.hash_user_valid(token_encode)
>> True
# or you can check if the limited token is already valid
temporary_token.hash_user_valid(token_limited)
>> True
# check the limit of the token:
temporary_token.hash_get_limit(token_limited)
>> datetime.datetime(2016, 10, 5, 21, 10, 6, 732771)
temporary_token.hash_get_limit(token_encode)
>> False  # return false because token_encode does not have limit
temporary_token.hash_get_uid(token_encode)
>> 3  # user id
```

Add extra security: (recommended):
==================================

If you want to add some extra security you can add an extra token
**Example:**
```python
from .... import TemporaryToken
from django.contrib.auth.models import User
user = User.objects.all().first()

temporary_token = TemporaryToken(user)
token_encode = temporary_token.hash_user_encode(hours_limit=2, extra_token="my-extra-custom-token")
temporary_token.hash_user_valid(token_encode, extra_token="my-extra-custom-token")
>> True
temporary_token.hash_user_valid(token_encode)
>> False # => If we forgot to add the extra token te response will be False
```
    
Second, Temporary Token Class Content
=============================
You can [download](https://github.com/pdonaire1/diccionario_de_comandos/blob/master/Django/temporary_token.py) 
the file or create a file ```temporary_token.py``` with the next content: 
    
```python
from django.contrib.auth.tokens import default_token_generator
from django.utils.encoding import force_bytes
from django.utils.http import urlsafe_base64_encode, urlsafe_base64_decode
import hashlib
import base64
import datetime
import dateutil.parser

class TemporaryToken:
    """
        Class for create limited or unlimited token from an user.
        created by: @pdonaire1 05-10-2016
    """
    def __init__(self, user):
        self.user = user
        
    def hash_user_encode(self, hours_limit=None, extra_token=''):
        """
            This method allow create a temporary or unlimited token 
            for users.
            hours_limit => int(24) for 1 day
            extra_token => a string value for more security
                This extra token not support the next caracter: "|"
        """
        uidb64 = urlsafe_base64_encode(force_bytes(self.user.pk))
        token = default_token_generator.make_token(self.user)
        date = datetime.datetime.now()
        date_str = date.isoformat()
        until = date + datetime.timedelta(hours=hours_limit) if hours_limit else 'unlimited'
        token_hash = '%s|%s|%s|%s|%s' % (
            uidb64, token, date_str, until, extra_token)
        return base64.urlsafe_b64encode(bytes(token_hash))
        
    def hash_user_valid(self, token, extra_token=''):
        """
            This method allow to check if token is valid either 
            temporary or unlimited.
        """
        token_split = token.split("=")
        # if the token is altered
        if (token_split[-1:][0] != "" or len(token_split) > 3
            or token_split[1] != ""): 
            return False
        try:
            token_hash = base64.urlsafe_b64decode(bytes(token))
        except: return False
        uidb64, token_decoded, date, limit, token_added = \
            token_hash.split('|')
        valid_uid = int(urlsafe_base64_decode(uidb64)) == \
            self.user.pk
        valid_token = token_decoded == \
            default_token_generator.make_token(self.user)
        present = datetime.datetime.now()
        if not self.hash_has_limit(token):
            limit = present + datetime.timedelta(minutes=1)
        else:
            limit = dateutil.parser.parse(limit)

        valid_extra_token = token_added == extra_token
        if (valid_uid and valid_token and (present < limit)
            and valid_extra_token):
            return True
        return False
    
    def hash_get_uid(self, token):
        """
            Method that return the user id
        """
        try:
            token_hash = base64.urlsafe_b64decode(bytes(token))
        except: return False
        uidb64 = token_hash.split('|')[0]
        return int(urlsafe_base64_decode(uidb64))

    def hash_get_extra_token(self, token):
        """
            Method that return the user id
        """
        try:
            token_hash = base64.urlsafe_b64decode(bytes(token))
        except: return False
        extra_token = token_hash.split('|')[4]
        return extra_token
    
    def hash_has_limit(self, token):
        """
            Method which allow know if token si limited
        """
        try: 
            token_hash = base64.urlsafe_b64decode(bytes(token))
        except: return False
        limit = token_hash.split('|')[3]
        if limit == 'unlimited':
            return False
        else:
            return True

    def hash_get_limit(self, token):
        """
            This function allow to check if token is valid either 
            temporary or unlimited.
            created by: @pdonaire1 05-10-2016
            limit => format '%b %d %Y %I:%M%p'
        """
        try:
            token_hash = base64.urlsafe_b64decode(bytes(token))
        except: return False
        limit = token_hash.split('|')[3]
        if limit == 'unlimited':
            return False
        if not self.hash_has_limit(token_hash):
            present = datetime.datetime.now()
            limit = present + datetime.timedelta(minutes=1)
        else:
            limit = dateutil.parser.parse(limit)
        return limit
```
