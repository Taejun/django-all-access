Release History
====================================

Release and change history for django-all-access


v0.5.0 (2013-03-18)
------------------------------------

This release adds additional hooks for changing the OAuth client behaviors. It also
adds support for Python 3.2+.

- New view hooks for customizing the OAuth client
- Fixed issue with including oauth_verifier in POST when fetching the access token
- Documented the API for :py:class:`OAuthClient` and :py:class:`OAuth2Client`
- Updated requirements to requests >= 1.0 and requests_oauthlib >= 0.3.0
- Updated requirement for PyCrypto >= 2.4

Backwards Incompatible Changes
__________________________________

- Dropped support for requests < 1.0
- Dropped support for Django < 1.4.2


v0.4.1 (2013-01-02)
------------------------------------

There were incompatibilty issues with requests-oauthlib (0.2) and requests which
required dropping requests 1.0 support. The requirement of oauthlib was also raised
to 0.3.4 due to similar issues. For more detail see the below issues.

- https://github.com/requests/requests-oauthlib/issues/1
- https://github.com/requests/requests-oauthlib/pull/10


v0.4.0 (2012-12-19)
------------------------------------

This release is largely to keep pace with features/changes to some of the
dependencies. This also helps work toward Python 3.0 support.

- Updated for compatibility with Django 1.4 timezone support
- Updated for compatibility with Django 1.5 swappable ``auth.User``
- Updated for compatibility with Requests 1.0
    - Added requests_oauthlib requirement
    - Updated requirement of oauthlib to 0.3 or higher


v0.3.0 (2012-07-13)
------------------------------------

This release added some basic logging to django-all-access. To enable this logging
in your project you should update your ``LOGGING`` configuration to include the
``allaccess`` in the ``loggers`` section. Below is an example:

.. code-block:: python

    LOGGING = {
        'handlers': {
            'console':{
                'level':'DEBUG',
                'class':'logging.StreamHandler',
            },
            'mail_admins': {
                'level': 'ERROR',
                'class': 'django.utils.log.AdminEmailHandler',
                'filters': ['special']
            }
        },
        'loggers': {
            'django.request': {
                'handlers': ['mail_admins', ],
                'level': 'ERROR',
                'propagate': True,
            },
            'allaccess': {
                'handlers': ['console', ],
                'level': 'INFO',
            }
        }
    }

For more information on logging please see the
`Django doucmentation <https://docs.djangoproject.com/en/1.4/topics/logging/>`_
or the `Python doucmentation <http://docs.python.org/library/logging.html>`_.


Features
_________________

- Added access to simple API wrapper through the ``AccountAccess`` model
- Added state parameter for OAuth 2.0 by default
- Added basic error logging to OAuth clients and views
- Added contributing guide and mailing list info


v0.2.1 (2012-06-29)
------------------------------------

Bug Fixes
_________________

- Fixes missing Content-Length header when requesting OAuth 2.0 access token


v0.2.0 (2012-06-24)
------------------------------------

There are two South migrations included with this release. To upgrade you should run::

    python manage.py migrate allaccess

If you are not using South you will not need to change your database schema because
the underlying field type did not change. However you should re-save all existing
``AccountAccess`` instances to ensure that their access tokens go through the encryption step

.. code-block:: python

    from allaccess.models import AccountAccess

    for access in AccountAccess.objects.all():
        access.save()


Features
_________________

- ``OAuthRedirect`` view can now specify a callback url
- ``OAuthRedirect`` view can now specify additional permissions
- Context processor for adding enabled providers to the template context
- User access tokens are stored with AES encryption
- Documentation on customizing the view workflow behaviors
- Travis CI integration

Bug Fixes
_________________

- Fixed OAuth2Client to include ``grant_type`` paramater when requesting access token
- Fixed OAuth2Client to match current OAuth draft for access token response as well as legacy response from Facebook


Backwards Incompatible Changes
__________________________________

- Moving the construction on the callback from the client to the view changed the signature of the client ``get_redirect_url``, ``get_redirect_args``, ``get_request_token`` (OAuth 1.0 only) and ``get_access_token`` to include the callback. These are largely internal functions and likely will not impact existing applications.
- The ``AccountAccess.access_token`` field was changed from a plain text field to an encrypted field. See previous note on migrating this data.


v0.1.1 (2012-06-22)
------------------------------------

- Fixed bug with passing incorrect callback parameter for OAuth 1.0
- Additional documentation on configuring ``LOGIN_URL`` and ``LOGIN_REDIRECT_URL``
- Additional view tests
- Handled poor ``LOGIN_URL`` and ``LOGIN_REDIRECT_URL`` settings in view tests


v0.1.0 (2012-06-21)
------------------------------------

- Initial public release.
