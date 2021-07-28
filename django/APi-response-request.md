## Request
### request.body value값을 쓰는 법
```python
data = json.loads(request.body) # JSONParser().parse(request)를 써도 무관
name = data['name']
```
### QueryParameter
```python
class ExView(APIView):
  def get(self, request):
    key = request.GET['key']  # request.GET.get('key',default=None)를 사용하면 key값이 없을때 null로 받아온다
    return Response(status=200)
```
* 쿼리를 보낼때 list 형식으로 가능
```
/url/?key=[1,3,4]
```
## Response
serializer.data로 변환하지 않고   
직접 Json형식으로 만들어 보내줄 수 있다 (list 형식?)
```python
return JsonResponse({'key':list(result_queryset)}, status=200)
```
* queryset을 합칠 땐 union 사용 ( **다른 모델이면 불가능** ) 
```python
return = qs1.union(qs2) # | 사용 시 오류 여부
```
* queryset.update()를 통해 여러 데이터를 수정 가능
```python
banner = Banner.objects.filter(name=event.brand.name)
banner.update(count=banner[0].count+1, )
```
참고 : https://wave1994.tistory.com/52
