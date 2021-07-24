### @action 데코레이터를 사용하여 viewset을 커스텀 할 수 있음
```python
class EventViewSet(viewsets.ModelViewSet):
    serializer_class = EventSerializer
    queryset = Event.objects.all()
    filter_backends = (DjangoFilterBackend,)
    filterset_class = EventFilter
    
    @action(detail=False, methods=['post'])
    def get_detail(self, request):
        data = JSONParser().parse(request)
        return HttpResponse(status=200)
```
* detail=True는 pk값을 받는 경우고, 목록을 받을 땐 detail=False
* methods 설정의 default는 GET
* 위 viewset 추가 매핑 함수의 url은 /prefix/get_detail/
