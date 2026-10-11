# Lab 05: API систем тест (Postman ба Newman)

**Оюутан:** Munh-Orgil, B242270130

```
$ node -v
v22.22.1
$ newman -v
6.2.2
```

## Тест дизайн (Даалгавар 2)

### Сонголтууд ба төлөөлөх утгууд

| Сонголт | Эквивалент анги | Төлөөлөх утга |
|---|---|---|
| studentID | байхгүй / идэвхтэй / идэвхгүй | `S_GHOST` / `status:"active"` / `status:"inactive"` |
| courseID | байгаа / байхгүй | `CS313` / `CS999` |
| Хичээлийн урьдач | байхгүй (`[]`) / бүгдийг үзсэн / зарим нь / огт үзээгүй | `[]` / `["CS201"]` үзсэн / `["CS201","CS202"]` нэгийг л үзсэн / `["CS201"]` огт үзээгүй |
| Хүсэлтийн формат | зөв / талбар дутуу / буруу JSON | бүрэн бие / `{"studentID":"X"}` / `{bad` |

### Спецификацууд

| # | Нөхцөл | Хүлээгдэх статус | Хүлээгдэх result |
|---|---|---|---|
| 1 | Happy path | 201 | OK + registrationID тоо |
| 2 | Оюутан байхгүй | 200 | ERROR_NO_STUDENT |
| 3 | Оюутан идэвхгүй | 200 | ERROR_INACTIVE_STUDENT |
| 4 | Хичээл байхгүй | 200 | ERROR_NO_COURSE |
| 5 | Урьдач огт үзээгүй | 200 | ERROR_PREREQUISITES, missing=["CS201"] |
| 6 | Зарим урьдач дутуу | 200 | ERROR_PREREQUISITES, missing=["CS202"] |
| 7 | Идэвхгүй оюутан + байхгүй хичээл | 200 | ERROR_INACTIVE_STUDENT |
| 8 | Байхгүй оюутан + байхгүй хичээл | 200 | ERROR_NO_STUDENT |
| 9 | Урьдачгүй хичээл, хоосон coursesTaken | 201 | OK |
| 10 | `courseID` талбар дутуу | 400 | ERROR_BAD_REQUEST |

## Newman ажиллуулалтууд (Даалгавар 4)

| Ажиллуулалт | Файл | assertions executed | failed | exit code |
|---|---|---|---|---|
| PASS | results/newman-pass.txt | 24 | 0 | 0 |
| FAIL | results/newman-fail.txt | 24 | 1 | 1 |
| DOWN | results/newman-down.txt | - | - | 1 |

Collection-д 10 бие даасан тест, 24 assertion байна.

ECONNREFUSED нь интерфейсийн (холболтын) алдаа: server ажиллаагүй тул хариу ирээгүй, oracle шалгагдаагүй. Харин FAIL ажиллуулалтад server хариу өгсөн боловч хүлээсэн 200 нь бодит 201-тэй таарсангүй, энэ нь oracle-ийн алдаа.

## Дүгнэлт

(8-10 өгүүлбэр, өөрөө бич)