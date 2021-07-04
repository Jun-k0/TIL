## 1. drf-yasg 설치
``` pip install drf-yasg ```
## 2. setting.py에 앱 추가
```python
INSTALLED_APPS = [
    ...
    'drf_yasg',
]
```
## 3. url.py에 내용 추가
```python
...
from drf_yasg.views import get_schema_view
from drf_yasg import openapi
from django.urls import path, include
from django.conf.urls import url
from rest_framework.permissions import AllowAny


schema_view_v1 = get_schema_view(
    openapi.Info(
        title="Open API",
        default_version='v1',
        description="간단한 설명",
        terms_of_service="https://www.google.com/policies/terms/",
    ),
    public=True,
    permission_classes=(AllowAny,), # 권한을 가진 사용자만 볼 수 있음
)


urlpatterns = [
	...

    url(r'^swagger(?P<format>\.json|\.yaml)$', schema_view_v1.without_ui(cache_timeout=0), name='schema-json'),
    url(r'^swagger/$', schema_view_v1.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    url(r'^redoc/$', schema_view_v1.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
]
```

* 이후 아래 주소로 접속하면 API 문서를 볼 수 있다.   
http://127.0.0.1:8000/redoc/   
http://127.0.0.1:8000/swagger/
