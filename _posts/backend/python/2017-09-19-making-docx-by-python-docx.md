---
layout: post
title:  "Making docx by python-docx"
categories: python
tags: python docx
---

파이썬에서 docx를 다루는 여러 라이브러리들(`pypandoc`, `docx`, `python-docx`) 중 star가 가장 많은 python-docx를 선택해 사용하였다. 기회가 된다면 다른 라이브러리를 사용해보는 것도 좋은 공부가 될 것 같다.

## Installing docx
`pip install python-docx`


## Using
사용법은 [공식문서](https://python-docx.readthedocs.io/en/latest/)에 잘 설명되어 있다. 

```python
from docx import Document
# import Inches using for add picture
from docx import Inches

# 문서를 선언
document = Document()

# Write title
document.add_heading('Document Title', 0)
# Write content
p = document.add_paragraph('This is first paragraph')
# 다음과 같이 p(문단) 뒤에 특정 글자만 추가해줄 수 있다.
p.add_run('by').bold = True
p.add_run('python-docx')
p.add_run('-sckim.').italic = True

# Write list format paragraph
document.add_paragraph(
	'some text for list',
	style='ListBullet' # 점 모양 리스트
	# style='ListNumber' # 숫자 리스트
)
# Add some pic
ducment.add_picture('name.png', width=Inches(1.25))
# Also can add tables
table = documetn.add_table(row=1, col=3)

hdr_cells = table.rows[0].cells
hdr_cells[0].text = 'Qty'
hdr_cells[1].text = 'Id'
hdr_cells[2].text = 'Desc'
for item in recordset:
    row_cells = table.add_row().cells
    row_cells[0].text = str(item.qty)
    row_cells[1].text = str(item.id)
    row_cells[2].text = item.desc

# Page Breaking
# Automatically page break
document.add_page_break()

# Saving
document.save('name.docx')
```

## Advanced usage

## 후기
python-docx를 사용해 보면서 잘 만들어진 완성도 높은 라이브러리라는 생각이 들었다. 특히나 python을 사용하여 docx로 다양한 형식의 수준 높은 파일 또한 쉽게 만들어 낼 수 있을 것 같다는 생각이 들었다. (하지만 누가 그렇게 만들까, word를 켜서 직접 gui로 만들고 말지...)

다만 다양한 기능을 갖추고 있기 때문에 그냥 string을 docx 형식으로 저장만 하는 상황에서는 굳이 전체 라이브러리가 필요하지 않다. 그냥 docx 형식으로 저장만 할 경우에는 다른 가벼운 라이브러리들도 충분히 좋을 것 같다. 
