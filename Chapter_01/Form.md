
# 📄 PowerApps Form 정리 문서

PowerApps에서 Form(폼)은 사용자가 데이터를 **입력, 보기, 수정**할 수 있도록 도와주는 핵심 UI 컨트롤입니다. 주로 SharePoint, Dataverse 등의 외부 데이터 소스와 연결해 사용합니다.

---

## ✅ Form 종류

| 폼 종류         | 설명 |
|------------------|------|
| `DisplayForm`    | 읽기 전용 폼. 데이터를 보기만 할 때 사용 |
| `EditForm`       | 데이터 읽기 + 편집 가능. 신규 등록, 수정 가능 |

> 💡 같은 폼에서도 `FormMode.View`, `FormMode.Edit`, `FormMode.New` 설정에 따라 다르게 동작합니다.

---

## 🧱 Form 구성 요소

| 구성 요소       | 설명 |
|------------------|------|
| **Form**         | 전체 폼 컨트롤. 데이터소스와 연결됩니다 |
| **DataCard**     | 각 데이터 필드별로 생성되는 개별 카드 컨트롤 |
| **DataCardValue**| 사용자 입력을 받는 실제 컨트롤 (예: TextInput) |

---

## ⚙️ 주요 속성

| 속성            | 설명 |
|------------------|------|
| `DataSource`     | 연결된 데이터 원본 |
| `Item`           | 표시할 데이터 레코드 (예: `Gallery1.Selected`) |
| `DefaultMode`    | 폼의 기본 모드 설정 (New, Edit, View) |
| `OnSuccess`      | 제출 성공 시 실행할 액션 |
| `OnFailure`      | 제출 실패 시 실행할 액션 |

---

## 🛠️ 폼 사용 예시 (Power Fx)

```powerfx
// 폼을 신규 입력 모드로 설정
NewForm(EditForm1);

// 폼을 수정 모드로 설정
EditForm(EditForm1);

// 폼을 보기 모드로 설정
ViewForm(EditForm1);

// 데이터 저장 (제출)
SubmitForm(EditForm1);
```
## 💡 팁
Patch() 함수를 사용하면 수동으로 저장도 가능하지만, SubmitForm()이 간편합니다.

각 DataCard의 Visible 속성을 조건부로 설정해 유동적인 폼을 만들 수 있습니다.

FormMode에 따라 같은 Form을 다르게 활용할 수 있습니다.

## 📚 추천 구성 방식 예시
```powerfx

If(
    IsBlank(ThisItem),
    NewForm(MyForm),
    EditForm(MyForm)
);
Navigate(FormScreen, Fade);
```

## 🧪 실습: 편집할 양식 액션 추가 및 새로 만들기 모드 설정

### 📌 목표
- 편집할 양식(EditForm)을 추가하고,
- 특정 화면의 우측 영역에 위치시킵니다.
- 데이터 소스는 `게시판`입니다.
- 기본 모드를 `New`(새 항목 생성)으로 설정합니다.

---

### 1 Form 컨트롤 추가
1. `삽입 > 양식 > 편집 양식(Edit Form)`을 선택합니다.
2. `Form1`의 **DataSource** 속성을 설정합니다:

```powerfx
DataSource = 게시판
```
## 3. 기본 모드를 새로 만들기로 변경합니다.

![image](https://github.com/user-attachments/assets/dc8742d3-b4bb-4849-8238-2d8b338e9bd4)

## 4. 필드 구성: 제목 및 내용만 추가
Form1을 선택한 상태에서 오른쪽 필드 편집(Edit Fields) 메뉴 클릭

모든 필드를 제거한 후, 아래 두 가지만 추가:

제목 (예: 제목 또는 Title)

내용 (예: 내용 또는 Body, Description 등 데이터 소스에 따라 다름)


![image](https://github.com/user-attachments/assets/3b3aa531-54a1-4d09-a0f6-4ab076270890)

## 5. 저장 버튼 추가 및 SubmitForm 실행
삽입 > 버튼 추가 → 텍스트: "저장"

버튼의 OnSelect 속성 입력:

```powerfx
SubmitForm(Form4)
```

## ▶️ 앱 실행
F5 키를 눌러 실행

## 📝 입력
제목과 내용을 입력합니다.

## ✅ 저장
"저장" 버튼을 클릭하여 데이터를 제출합니다.


## 6 입력 완료 후 폼 초기화 처리
✅ 이유
SubmitForm만 실행하면 폼은 이전 입력값을 유지합니다.

새 입력을 위해 폼을 초기화해야 재사용이 가능합니다.

## 🛠 처리 방법
저장 버튼의 OnSelect 속성에 아래처럼 추가:

```SubmitForm(Form4);
ResetForm(Form4);
```
