### admin을 커스텀하여 디자인 및 기능을 추가할 수 있다
```python
@admin.register(Brand)  # admin에 등록
class BrandAdmin(admin.ModelAdmin):
    actions = ['make_banner_brand']   # 기능 추가
    # list_display = ['id', 'title', 'status', 'created_at', 'updated_at' ] # 보여질 필드 목록

    def make_banner_brand(self, request, queryset): # 추가할 기능 (action)
        for i in queryset:
            Banner.objects.create(name=i.name, count=0)

    make_banner_brand.short_description = '지정 브랜드의 배너 브랜드 추가'   # action 이름

```
