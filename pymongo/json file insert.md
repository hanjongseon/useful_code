
```python

response = requests.get("https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/restaurants.json")
#url 상으로 존재하는 json 파일

#실질적으로 그냥 다운로드 받는게 더 편함
open(r"g:/내 드라이브/work/restaurants.json", 'wb').write(response.content)

requesting = []  

#인코딩 맞춰서 오픈해준 후에.
with open(r"g:/내 드라이브/work/restaurants.json", encoding='utf8') as f:
    for jsonObj in f:
        myDict = json.loads(jsonObj)
        #가끔 $oid라는 것이 있는데
        # 파이썬의 문제인지는 모르겠으나 objectId로 바꿔줘야 깔끔하게 올라감.
        # 아닐 경우 에러발생
        # objectId로 복사하고, 기존키 삭제
        myDict['_id']["objectId"] = myDict['_id']["$oid"]
        del myDict['_id']["$oid"]
        requesting.append(InsertOne(SON(myDict)))# 파이프라인 계속만들어줌

result = db.restaurants.bulk_write(requesting)

```