To create tokens temporary or unlimited by user you can use this class or you can modify it

    from django.contrib.auth.tokens import default_token_generator
    from django.utils.encoding import force_bytes
    from django.utils.http import urlsafe_base64_encode, urlsafe_base64_decode
    import hashlib
    import base64
    import datetime
    import dateutil.parser

    def hash_user_encode(user, hours_limit=None):
        """
            This function allow create a temporary or unlimited token 
            for users.
            created by: @pdonaire1 05-10-2016
        """
        uidb64 = urlsafe_base64_encode(force_bytes(user.pk))
        token = default_token_generator.make_token(user)
        date = datetime.datetime.now().isoformat()
        until = date + timedelta(hours=hours_limit) if hours_limit else 'unlimited'
        hash = '%s|%s|%s|%s' % (uidb64, token, date, until)
        return base64.urlsafe_b64encode(bytes(hash))

    def hash_user_check(user, token):
        """
            This function allow to check if token is valid either 
            temporary or unlimited.
            created by: @pdonaire1 05-10-2016
            limit => format '%b %d %Y %I:%M%p'
        """
        hash = base64.urlsafe_b64decode(bytes(token))
        uidb64, token, date, limit = hash.split('|')
        valid_uid = int(urlsafe_base64_decode(uidb64)) == user.pk
        valid_token = token == default_token_generator.make_token(user)
        present = datetime.datetime.now()
        if date == 'unlimited':
            date = present + timedelta(minutes=1)
        else:
            date = dateutil.parser.parse(date)
        if valid_uid and valid_token and (present < date):
            return True
        return False
