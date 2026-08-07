# Common Specification (Common Spec)

Tài liệu tổng hợp các quy tắc dùng chung (Common Rules - CMR) trong hệ thống.

---

## CMR01: Paging Infinite Scroll

### 📝 Common Rule Detail (English / General)
```text
Flow:
1)Initial Load:
When the screen or component is loaded for the first time:
   +Call the API to retrieve the first page (page = 1).
   +Display the results in the list 20 record per page.

2)On User Scroll:
When the user scrolls near the bottom of the list (e.g., 80–90% of the list height):
    +Call the API for the next page (page = current Page + 1).
    +Append the new data to the existing list.
    +Increment current Page.

UI States
Show a loading spinner when fetching the next page if loading perfomance is slow

Cases & Handling
- New data (added after the current search) will not appear immediately — it will only be shown after the user performs a new search. Each new search will refresh the result list and reset pagination to the first page.
- Slow network or API failure: Show error message CMMSG2
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
フロー：
1) 初期読み込み：
画面またはコンポーネントが初めて読み込まれたとき：
+ API を呼び出して最初のページ（ページ = 1）を取得します。
+ リストに結果を表示します（1 ページあたり 20 件のレコード）。

2) ユーザースクロール時：
ユーザーがリストの下部付近（例：リストの高さの 80～90%）までスクロールしたとき：
+ 次のページ（ページ = 現在のページ + 1）の API を呼び出します。
+ 新しいデータを既存のリストに追加します。
+ 現在のページを 1 つ増やします。

UI の状態
読み込み速度が遅い場合、次のページを取得する際に読み込みスピナーを表示します。

ケースと処理
- 新しいデータ（現在の検索後に追加されたデータ）はすぐには表示されず、ユーザーが新しい検索を実行した後にのみ表示されます。新しい検索を実行するたびに結果リストが更新され、ページ区切りが最初のページにリセットされます。
- ネットワーク速度が遅い場合または API に障害が発生した場合：エラーメッセージ CMMSG2 を表示します。
```

---

## CMR02: Search Box

### 📝 Common Rule Detail (English / General)
```text
Action:
- Hit Enter to perform searching
- Disable searching if the input is empty: Pressing Enter won’t navigate to the result screen.
- When text is being entered, a transparent "X" icon will appear to allow users to clear the text.

Partial Searches: 
- Support partial matches (e.g., typing "conta" matches “Contact”)
- Case-insensitive matching

Fallback Message: show "一致する検索結果はありません。" when there is no results

Supports all characters: including all languages and special characters (including emojis)
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
アクション:
- Enter キーを押して検索を実行します。
- 入力内容が空の場合は検索を無効にします。Enter キーを押しても結果画面には移動しません。
- テキスト入力中は透明な「X」アイコンが表示され、ユーザーはテキストをクリアできます。

部分一致検索:
- 部分一致をサポートします（例: 「conta」と入力すると「Contact」が一致します）
- 大文字と小文字を区別しない一致

フォールバックメッセージ: 結果がない場合は「一致する検索結果はありません。」と表示します。

すべての文字をサポートします（すべての言語と特殊文字（絵文字を除く）を含む）
```

---

## CMR03: Label

### 📝 Common Rule Detail (English / General)
```text
1. Objective
Display static text, such as titles, descriptions, or hints, in the user interface (UI) without any interactive behavior

2. Scope
-Used across the entire application (web/app) for static UI content, such as:
-Form titles
-Instructional hints
-Terms and policy descriptions
-Support notes
-Not used for buttons, links, or any interactive elements.

3. Core Functionalities
-Display non-tabable, non-interactive text
-Render label text according to the current system language
-Automatically update text when the language is changed by the user
-Support dynamic content insertion (e.g., using variables like username)
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. 目的
タイトル、説明、ヒントなどの静的テキストを、インタラクティブな動作を伴わずにユーザーインターフェース（UI）に表示する

2. 適用範囲
- アプリケーション全体（ウェブ/アプリ）で、以下のような静的UIコンテンツに使用します。
- フォームのタイトル
- 操作のヒント
- 利用規約とポリシーの説明
- サポートノート
- ボタン、リンク、その他のインタラクティブな要素には使用しません。

3. コア機能
- クリック不可でインタラクティブではないテキストを表示する
- 現在のシステム言語に従ってラベルテキストをレンダリングする
- ユーザーが言語を変更したときにテキストを自動的に更新する
- 動的なコンテンツの挿入をサポートする（例：ユーザー名などの変数の使用）
```

---

## CMR04: Input Validation Rule Spec

### 📝 Common Rule Detail (English / General)
```text
-No frontend validation with error messages.
-Instead, proactively block invalid actions (numeric input) at the UI level (blocking UX).

Numeric Input Field:
-Allow only numeric characters (0-9)
-Disallow letters, special characters, and spaces        
-Block invalid characters immediately as they are typed, without showing error messages
-Message from backend if needed: CMMSG54

Required Field
-If the field is empty → still enable related button or action but return message from backend      

Max Length
-No real-time validation required; check this on submit or server-side
        
Min Length (if any)
-No real-time validation required; check this on submit or server-side
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- フロントエンドでの検証によるエラーメッセージは不要です。
- 代わりに、UIレベルで無効なアクション（数値入力）を積極的にブロックします（UXを阻害します）。

数値入力フィールド：
- 数字（0～9）のみを許可します。
- 文字、特殊文字、スペースは許可しません。
- 無​​効な文字は入力時に直ちにブロックし、エラーメッセージは表示しません。

必須フィールド
- フィールドが空の場合 → 関連ボタンまたはアクションは有効ですが、バックエンドからメッセージが返されます。

最大文字数
​​- リアルタイム検証は不要です。送信時またはサーバー側でチェック

最小文字数（必要な場合）
- リアルタイム検証は不要です。送信時またはサーバー側でチェック
```

---

## CMR05: Multi-Language Support

### 📝 Common Rule Detail (English / General)
```text
Ensure consistent handling of multi-language content throughout the application, making it scalable, maintainable, and localization-friendly.
- When login, display previously selected language.
-Apply to all screen


Text Resources        
-All text (UI labels, messages, placeholders, button texts, error messages, etc.) must be managed via a centralized language resource file or localization system

Default Language
-Define a system default language Japanese to fall back to when no translation is available.

Dynamic Values
Support dynamic placeholders in translations (e.g. Hello, {name}!) and avoid hardcoded string concatenation.

Font Support
Ensure fonts used in the UI support all languages and special characters

Error Handling:
-If a translation key is missing, display the default language text or a clearly identifiable fallback (e.g. !!missing_key!!) for debugging.
-Avoid displaying empty strings.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
アプリケーション全体で多言語コンテンツの処理を統一し、スケーラビリティ、保守性、ローカリゼーションの容易性を高めます。
- ログイン時に、以前に選択した言語を表示します。
- すべての画面に適用

テキストリソース
- すべてのテキスト（UI ラベル、メッセージ、プレースホルダ、ボタンテキスト、エラーメッセージなど）は、一元化された言語リソースファイルまたはローカリゼーションシステムで管理する必要があります。

デフォルト言語
- 翻訳が利用できない場合にフォールバックするシステムのデフォルト言語として日本語を定義します。

動的な値
翻訳で動的なプレースホルダ（例：Hello, {name}!）をサポートし、ハードコードされた文字列の連結は避けます。

フォントサポート
UI で使用されるフォントがすべての言語と特殊文字をサポートしていることを確認します。

エラー処理：
- 翻訳キーが見つからない場合は、デバッグ用にデフォルト言語のテキストまたは明確に識別できるフォールバック（例：!!missing_key!!）を表示します。
- 空の文字列を表示しないようにします。
```

---

## CMR06: Common Message & Error Response

### 📝 Common Rule Detail (English / General)
```text
1-Scope
-Applies to all API responses from backend servers to:
+Web Frontend
+Mobile Applications

2-Response Rules
-Clients must not hard-code any message locally.
-All message content is managed by backend.
-Backend maintains config file mapping message code to message text in each supported language.
-Clients should gracefully handle unknown codes or fallback to default message (JA).
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1-適用範囲
- バックエンドサーバーから以下のすべてのAPIレスポンスに適用されます。
+Webフロントエンド
+モバイルアプリケーション

2-レスポンスルール
- クライアントはメッセージをローカルにハードコードしてはなりません。
- すべてのメッセージコンテンツはバックエンドによって管理されます。
- バックエンドは、サポートされる各言語のメッセージコードとメッセージテキストをマッピングする設定ファイルを維持します。
- クライアントは不明なコードを適切に処理するか、デフォルトメッセージ（JA）にフォールバックする必要があります。
```

---

## CMR07: Back Button Functionality – Navigate to the Previous Screen

### 📝 Common Rule Detail (English / General)
```text
1. Objective
Allow users to return to the previous screen by pressing the “Back” button (either in-app or device), ensuring smooth and intuitive navigation flow.

2. User interactions:
-Tap the in-app "Back" button (UI)
-Press the device's physical "Back" button (on Android)
-Swipe-back gesture (on iOS)

3-If it's a form screen, show a popup CMMSG46
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. 目的
ユーザーが「戻る」ボタン（アプリ内またはデバイス内）を押すことで前の画面に戻れるようにし、スムーズで直感的なナビゲーションフローを実現します。

2. ユーザーインタラクション：
- アプリ内の「戻る」ボタンをタップする（UI）
- デバイスの物理的な「戻る」ボタンを押す（Androidの場合）
- スワイプバックジェスチャー（iOSの場合）

3. フォーム画面の場合は、ポップアップを表示する CMMSG46
```

---

## CMR08: Pulldown Menu (Dropdown) single value

### 📝 Common Rule Detail (English / General)
```text
1. Objective
Allow users to select a single value from a list of options displayed in a pulldown menu (dropdown), ensuring a smooth and intuitive user experience.

2. Input
-List of values: A list of available values for the user to choose from
-User interactions:
+tab on the dropdown field (input area).
+Choose a value from the displayed list.
+The list should be scrollable if the options exceed the visible area.

3. Expected Behavior
-Open list: When the user tabs on the dropdown field, the list of options should be displayed.
-Select a value: The user can select only one value at a time. Once selected, the value is displayed in the input field.
-Close list: The list should close automatically once a value is selected, or if the user tabs outside the dropdown.
-Display selected value: After selection, the value is shown in the input field rather than the list of options.
-Single selection: The dropdown should only allow one value to be selected at a time.

4. Actions
-Select a value: Once a user selects a value, the value should be displayed in the input field.
-Cancel/Reset: Provide an option to cancel the selection or reset the dropdown to its initial state.
-Default value: If there’s a default value, it will be shown when the dropdown is opened.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. 目的
プルダウンメニュー（ドロップダウン）に表示される選択肢のリストから、ユーザーが単一の値を選択できるようにすることで、スムーズで直感的なユーザーエクスペリエンスを実現します。

2. 入力
- 値のリスト：ユーザーが選択できる値のリスト
- ユーザーインタラクション：
+ ドロップダウンフィールド（入力領域）をクリックします。
+ 表示されたリストから値を選択します。
+ 選択肢が表示領域を超える場合は、リストをスクロールできるようにします。

3. 期待される動作
- リストを開く：ユーザーがドロップダウンフィールドをクリックすると、選択肢のリストが表示されます。
- 値を選択する：ユーザーは一度に1つの値のみを選択できます。選択した値は入力フィールドに表示されます。
- リストを閉じる：値が選択されるか、ユーザーがドロップダウンの外側をクリックすると、リストは自動的に閉じます。
- 選択された値を表示する：選択した値は、選択肢のリストではなく入力フィールドに表示されます。
- 単一選択：ドロップダウンでは、一度に1つの値のみを選択できるようにする必要があります。

4. アクション
- 値を選択：ユーザーが値を選択すると、入力フィールドにその値が表示されます。
- キャンセル/リセット：選択をキャンセルするか、ドロップダウンを初期状態にリセットするオプションを提供します。
- デフォルト値：デフォルト値が設定されている場合は、ドロップダウンを開いたときに表示されます。
```

---

## CMR09: Inital Form Input Display

### 📝 Common Rule Detail (English / General)
```text
1. Initial Display (First Time Use)
Display default values if available, otherwise show placeholder text.


2. Edit Mode (Updating Existing Data)
Pre-fill fields with previously entered values from API or local storage.
Show the character limit and live character count while typing.

3. Autofocus First Editable Field
Automatically focus on the first editable field when the form is opened.

4. Press Back Without Saving Changes
Show confirmation dialog: MSG2

5. Save Button Pressed
Validate all fields → If valid, call API → Show SUCCESS message depend on action and navigate back.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. 初期表示（初回使用時）
デフォルト値がある場合は表示し、ない場合はプレースホルダーテキストを表示します。

2. 編集モード（既存データの更新）
APIまたはローカルストレージから以前に入力された値で、フィールドに事前入力します。
入力中に文字数制限と有効な文字数を表示します。

3. 最初の編集可能なフィールドに自動フォーカス
フォームを開いたときに、最初の編集可能なフィールドに自動的にフォーカスします。

4. 変更を保存せずに「戻る」ボタンを押す
確認ダイアログ（MSG2）を表示します。

5. 保存ボタンが押された
すべてのフィールドを検証 → 有効な場合はAPIを呼び出し → アクションに応じて成功メッセージを表示し、前に戻ります。
```

---

## CMR10: Upload Image - Client side handle, Server side handle

### 📝 Common Rule Detail (English / General)
```text
Client side handle
-Resize handle 
 + downscaling the image while maintaining its original aspect ratio so that neither width nor height exceeds the defined max. Example: User uploads a 4000 x 3000px image → resized to 1024 x 768px (maintaining 4:3 aspect ratio) 
 + For screens where a fixed 1:1 aspect ratio has been decided for the profile avatar, the image will be resized to 1024x1024 at the image cropping stage. 

Flow
- User selects new avatar image
- App/web request Server for signed URL
- Server request S3 for signed URL with token and key 
- S3 return signed URL to Server, then Server return to App/web 

- App/web upload image using signed URL to S3, S3 
  →Upload sucess: 
       +S3 response upload success
       +App/web save new image path to user profile
               →If save image path success: 
　　　　　　-Sever send request delete old image
               →If save image path failed: 
　　　            -Sever return error message: CMMSG23　
                        -Retry to save image path

  →Upload failed:
       +S3 response upload failed: CMMSG23
       +Retry to upload image

Format S3 upload image:
user_profile/{uid}/avatar/xxx.png
user_profile/{uid}/background/yyy.png
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
クライアント側の処理
リサイズ処理
　+ 元のアスペクト比を維持したまま、幅または高さが定義された最大サイズを超えないように画像を縮小します。
　　例：ユーザーが 4000x3000px の画像をアップロードした場合 → アスペクト比 4:3 を維持したまま 1024x768px にリサイズされます。
　+ プロフィールアバターなどで 1:1 の固定アスペクト比が決められている画面では、クロップ時に 1024x1024 にリサイズされます。

フロー
- ユーザーが新しいアバター画像を選択
- アプリ/WEB がサーバーに署名付きURLをリクエスト
- サーバーが S3 にトークンとキーを使って署名付きURLをリクエスト
- S3 が署名付きURLをサーバーに返し、サーバーがそれをアプリ/WEB に返す


アップロード処理（署名付きURLを使用）
✅ アップロード成功時:
S3 がアップロード成功を返す

アプリ/WEB が新しい画像パスをユーザープロフィールに保存
　→ 保存成功時：
　　- サーバーが古い画像の削除リクエストを S3 に送信
　→ 保存失敗時：
　　- サーバーがエラーメッセージ CMMSG23 を返す
　　- 画像パスの保存をリトライ

❌ アップロード失敗時:
-S3 がアップロード失敗を返す：CMMSG23
-アプリ/WEB が画像アップロードを再試行

S3 アップロードパスフォーマット:
user_profile/{uid}/avatar/xxx.png  
user_profile/{uid}/background/yyy.png
```

### 🔗 Metadata & Technical Details
- **Flow Link:** [https://drive.google.com/drive/folders/15YSJHL1YlCzgKLe0Pk4-F6QkgcQYvy7y](https://drive.google.com/drive/folders/15YSJHL1YlCzgKLe0Pk4-F6QkgcQYvy7y)

---

## CMR11: Input Field

### 📝 Common Rule Detail (English / General)
```text
First time opening the form
- Display placeholder describing the field
- If a default value exists → display it
- No error or validation message shown initially

Revisiting for editing
- Show previously entered values
- Fields are editable (enabled)

Save Button Action
-Not pass validate rule: show validation errors from backend
-All inputs valid: 
  +Submit data to backend
  +Show loading / success / error state
- Show confirmation message
- Navigate back

Additional Handling
-Show character counter if needed
-Auto-format input (e.g., phone, date)  if needed
-Use correct keyboard types (e.g., numeric, email)
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
フォームを初めて開いたとき
- フィールドを説明するプレースホルダを表示する
- デフォルト値が存在する場合 → 表示する
- 最初はエラーや検証メッセージは表示しない

編集のために再度アクセスする
- 以前に入力した値を表示する
- フィールドは編集可能（有効）

保存ボタンのアクション
- 検証ルールに合格しない場合：バックエンドからの検証エラーを表示する
- すべての入力が有効：
+ バックエンドにデータを送信
+ 読み込み中 / 成功 / エラー状態を表示する
- 確認メッセージを表示する
- 戻る

追加の処理
- 必要に応じて文字数カウンタを表示する
- 必要に応じて入力（例：電話番号、日付）を自動フォーマットする
- 正しいキーボード入力タイプを使用する（例：数字、メールアドレス）
```

---

## CMR12: Rich Text Editor Input Field

### 📝 Common Rule Detail (English / General)
```text
Tool: CKEditor 5

Input:
Users can enter rich text including formatted text, links, images, tables, and other CKEditor 5-supported content.

Validation:
-Must not be empty (if required).

Main Features:
-Text formatting (bold, italic, underline, strikethrough)
-Bullet and numbered lists
-Insert images and links
-Tables
-Undo/redo
-Multi-language input supported (không lỗi font là ok)
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
ツール: CKEditor 5

入力:
ユーザーは、書式付きテキスト、リンク、画像、表、その他CKEditor 5でサポートされているコンテンツを含むリッチテキストを入力できます。

検証:
- 必須の場合、空欄にすることはできません。

主な機能:
- テキストの書式設定 (太字、斜体、下線、取り消し線)
- 箇条書きと番号付きリスト
- 画像とリンクの挿入
- 表
- 元に戻す/やり直し
- 多言語入力に対応
```

---

## CMR13: Select / Pulldown single

### 📝 Common Rule Detail (English / General)
```text
-Display a list of predefined options for the user to select from. 

Initial Display
-First-time / Unselected: Show placeholder as desgin 
-When selected: Display the label of the selected item        

User Interaction Behavior
-User focuses the component: Open dropdown (mobile) or highlight the component (web)
-User selects an item: Display selected label, close dropdown
-User taps same item again: No change
-Clear selection (if needed): a "✕" button can be shown to clear the selection
-Disabled state (if needed): Component is disabled, no interaction possible
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- ユーザーが選択できる、定義済みのオプションのリストを表示します。

初期表示
- 初回 / 未選択時: プレースホルダをデザインとして表示
- 選択時: 選択された項目のラベルを表示

ユーザーインタラクションの動作
- ユーザーがコンポーネントにフォーカスを当てた場合: ドロップダウンを開く (モバイル)、またはコンポーネントをハイライト表示 (Web)
- ユーザーが項目を選択した場合: 選択されたラベルを表示し、ドロップダウンを閉じる
- ユーザーが同じ項目を再度タップした場合: 変化なし
- 選択をクリア (必要な場合): 選択をクリアするための「✕」ボタンを表示できます
- 無効状態 (必要な場合): コンポーネントは無効で、インタラクションは実行できません
```

---

## CMR14: Datepicker

### 📝 Common Rule Detail (English / General)
```text
Initial Display
-No selection: Show place holder
-Date selected: Display the selected date in localized format yyyy/mm/dd  hh:mm (Hide components depending on each screen.)
-With default value: If a default date is pre-filled, display it accordingly

User Interaction Behavior
-Tap or tab on field: Open date picker
-Date selected: Selected date is shown in the field, picker closes
-Clear date: A clear (“✕”) button is displayed to reset the field
-Cancel without selecting: Keep previous value or leave empty

Calendar UI
-Use native picker for mobile, custom calendar UI for web
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
初期表示
-選択なし：プレースホルダーを表示
-日付選択：選択された日付をローカライズされた形式（YYYY-MM-DD）で表示
-デフォルト値あり：デフォルトの日付があらかじめ入力されている場合は、それに従って表示

ユーザーインタラクションの動作
-フィールドをタップまたはクリック：日付ピッカーを開く
-日付選択：選択された日付がフィールドに表示され、ピッカーが閉じる
-日付をクリア：クリア（「✕」）ボタンが表示され、フィールドがリセットされる
-選択せずにキャンセル：前の値を保持するか、空白のままにする

カレンダーUI
-モバイルではネイティブピッカー、WebではカスタムカレンダーUIを使用する
```

---

## CMR14A: Date time format

### 📝 Common Rule Detail (English / General)
```text
yyyy/mm/dd  hh:mm (Hide components depending on each screen.)
```

### 🔗 Metadata & Technical Details
- **Value JP:** `yyyy/mm/dd  hh:mm`
- **Value EN:** `dd/mm/yyyy  hh:mm`

---

## CMR15: Submit Form Button

### 📝 Common Rule Detail (English / General)
```text
Functional Behavior:
-On tab: submit form data to API
-Validation: Except image/file upload, validation is handled by backend.
-Show loading indicator while API is processing.
-Disable the button while loading to avoid multiple submissions.
-Display field-level or global errors returned from API
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
機能の動作：
-クリック時：フォームデータをAPIに送信します。
-検証：画像/ファイルのアップロードを除き、検証はバックエンドで処理されます。
-API処理中は読み込み中インジケーターを表示します。
-複数回の送信を防ぐため、読み込み中はボタンを無効にします。
-APIから返されたフィールドレベルまたはグローバルエラーを表示します。
```

---

## CMR16: Avatar

### 📝 Common Rule Detail (English / General)
```text
Refer: CMR16
```

---

## CMR16(2): Active Status

### 📝 Common Rule Detail (English / General)
```text
BỎ YÊU CẦU NÀY 
Active Status: 
1-Not livestreaming

2-Livestream
-show icon streaming
Gif icon: https://drive.google.com/file/d/17qQ0NYQz4pfnDzVvcF6XWlp8KBKpsI6_/view
```

### 🔗 Metadata & Technical Details
- **API Mapping:**
```text
1.is _online
1.is_livesream
1.last_active

or
2.is _online
2.is_livesream
2.last_active
```
- **Swagger Link:**
```text
/users/me/                        
/users/{id}
```

---

## CMR17: User's Name

### 📝 Common Rule Detail (English / General)
```text
Name is shown in a single line; overflow is truncated with.
```

---

## CMR18: Follow Button/ Unfollow Button

### 📝 Common Rule Detail (English / General)
```text
1-Allow users to follow Other user they like and receive notifications when those users go live or post new content.

2. Basic Behavior
-Follow: When tapping the “Follow” button (on profile or livestream screen or list user): 
  +Save follow status
  +The Follow button updates to: “Following フォロ中”
  +Display a popup asking if user want to enable notification MSG20: 
    ・  Yes: Close the popup, Save the user’s preference to enable all notifications for {Username} (e.g., livestream, new post)
    ・   No: Close the popup, Save the user’s preference to disable all notifications for {Username}

-unFollow: When tapping the “unFollow” button (on profile or livestream screen or list user): 
  +Save follow status and automatically disable notification for that user

3. Orther logic 
(The follow logic will be described in detail within the corresponding screen logic.)

-suggest users commonly followed by others
-suggest livestreams with related content or Middle Categorys similar to the users you follow
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. ユーザーが気に入ったユーザーをフォローし、そのユーザーがライブ配信を開始したり新しいコンテンツを投稿したりした際に通知を受け取ることを可能にします。

2. 基本動作
-フォロー: プロフィール画面、ライブ配信画面、またはユーザーリストの「フォロー」ボタンをタップした時:
+フォローステータスを保存
+フォローボタンの表示が「フォロー中」に更新されます。
+通知を有効にするかどうかを尋ねるポップアップメッセージを表示します。MSG20:
・はい: ポップアップを閉じ、ユーザーの設定を保存して、{Username} のすべての通知（例: ライブ配信、新しい投稿）を有効にします。
・いいえ: ポップアップを閉じ、ユーザーの設定を保存して、{Username} のすべての通知を無効にします。

-フォロー解除: プロフィール画面、ライブ配信画面、またはユーザーリストの「フォロー解除」ボタンをタップした時:
+確認ダイアログを表示:「フォローを解除してもよろしいですか？」
+ フォローステータスを保存し、そのユーザーの通知を自動的に無効にする

3. その他のロジック
(フォローロジックについては、該当する画面ロジックで詳しく説明します。)

- 他のユーザーがよくフォローしているユーザーを提案する
- フォローしているユーザーに関連するコンテンツやトピックを含むライブ配信を提案する
```

### 🔗 Metadata & Technical Details
- **API Mapping:**
```text
API 1 
Follow : /follow/

API 2 
Unfollow : /follow/

API 3:
Block all notification from sender: /user-setting/notification/block-receiver/

API 4:
Unblock all notification from sender: /user-setting/notification/unblock-receiver/
```
- **Swagger Link:**
```text
API 1 
Follow : https://api-dev.surrealdolls.com/docs#/User%20follow/follow_user_follow__post

API 2 
Unfollow : https://api-dev.surrealdolls.com/docs#/User%20follow/unfollow_user_follow__delete

API 3
Block notificaiton: https://api-dev.surrealdolls.com/docs#/Users%20Settings/block_user_notification_user_setting_notification_block_receiver__post

API 4
Unblock notification : https://api-dev.surrealdolls.com/docs#/Users%20Settings/unblock_user_notification_user_setting_notification_unblock_receiver_post
```
- **Param:**
```text
API 1 
user_id : 10,
API 2 
user_id
API 3
sender_id (id của user mà bản thân vừa follow)
API 4
sender_id (id của user mà bản thân vừa follow)
```

---

## CMR19: Live Stream Thumbnail

### 📝 Common Rule Detail (English / General)
```text
CMR19

 CMR19 - New
```

---

## CMR20: Navigation Bar

### 📝 Common Rule Detail (English / General)
```text
- Is displayed at the top of the page on all screens.
- Highlight the current active route
- tab to open sections
- Is alway at the top even when scrolling down
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- すべての画面でページ上部に表示されます。
- 現在アクティブなルートをハイライト表示します。
- クリックするとセクションが開きます。
- 下にスクロールしても常に上部に表示されます。
```

---

## CMR21: Report

### 📝 Common Rule Detail (English / General)
```text
- To allow users to report inappropriate or harmful content, behavior, or accounts to admin

Placements:
- Post
- Livestream
- Livestream's comment
- User profile
- Chat messages (DM)

Action: When tapped, opens a report form or modal with selectable reasons depends on where the button is placed

Submission flow:
- User selects reason (option to attach media files like image or video)
- Submits report
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- ユーザーが不適切または有害なコンテンツ、行動、またはアカウントを管理者に報告できるようにするため

配置:
- 投稿
- ライブ配信
- ライブ配信のコメント
- ユーザープロフィール
- チャットメッセージ (DM)

アクション: タップすると、理由を選択できる報告フォームまたはモーダルが開きます （画面によっては、通報内容が異なる）

報告フロー:
- ユーザーが理由を選択（動画や画像添付可能）
- 報告を送信 → ブロックするオプションを含む確認メッセージが表示される
```

---

## CMR22: Post Thumbnail and Title

### 📝 Common Rule Detail (English / General)
```text
CMR22
```

---

## CMR23: Pagination Event

### 📝 Common Rule Detail (English / General)
```text
- Upon tab:
 +「前」：Navigate to the previous page (current page number minus 1)
 +  {Page number}: Navigate to the corresponding page number
 +「．．．」：Open a small input box and navigate to the corresponding page number
 +「次」：Navigate to the next page (current page number plus 1)

- Highlight the current page number

- Disable “Previous” （前）on first page, “Next”（次） on last page

- Display:
1. When the total number of page is 1~7 pages: Display all the page number, do not show [...].
Ex:
If there are 7 pages:  
　<<前　1　2　3　4　5　6　7　次>>
If there is 1 page (disable "Previous" and "Next"):   　
　<<前　1　次>>

2. When the number of page is above 7: Display page number 1~5, [...] and the last page number. 
Ex: If there are 11 pages: 
　<<前　1　2　3　4　5　…　11　次>>
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- クリック時:
+「前」：前のページへ移動（現在のページ番号から1を引いたページ）
+{ページ番号}：対応するページ番号へ移動
+「．．．」：小さな入力ボックスを開き、対応するページ番号へ移動
+「次」：次のページへ移動（現在のページ番号に1を足したページ）

- 現在のページ番号を強調表示

- 最初のページでは「前」、最後のページでは「次」を無効

- 表示仕様::
1. ページ数が1〜7ページの場合：すべてのページ番号を表示し、「…」は表示しない。
　例：
　ページ数が7の場合：　<<前　1　2　3　4　5　6　7　次>>
　ページ数が1の場合（「前」と「次」が無効）：　<<前　1　次>>

2. ページ数が7ページを超える場合：1〜5ページ、末尾のページ番号を表示し、「…」を使用する。
　例：
　ページ数が11の場合：　<<前　1　2　3　4　5　…　11　次>>
```

---

## CMR24: Check box

### 📝 Common Rule Detail (English / General)
```text
- Purpose: Allow users to select one or more options from the list 

- States: 
 + Unchecked (default)
 + Checked

- Behaviour:
 + tab toggles between checked and unchecked
 + "Select all" logic: If the parent is checked, all children become checked (same with unchecked)

- Delete button:
 + When one or more checkboxes are checked: Show the “Delete” button  
 + When no checkboxes are checked: Hide the “Delete” button
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- 目的: ユーザーがリストから1つ以上のオプションを選択できるようにする

- 状態:
+ 未チェック (デフォルト)
+ チェック済み

- 動作:
+ クリックすると、チェック済みと未チェックが切り替わります
+ 「すべて選択」ロジック: 親がチェックされている場合、すべての子がチェック済みになります (未チェックの場合も同様)

- 削除ボタン:
+ 1つ以上のチェックボックスがチェックされている場合: 「削除」ボタンを表示します
+ チェックボックスが1つもチェックされていない場合: 「削除」ボタンを非表示にします
```

---

## CMR25: Delete button

### 📝 Common Rule Detail (English / General)
```text
1. tabing “Delete” open a confirmation modal

2. Delete confirmation modal
- tabing the "X" button or the cancel button:
+ Immediately cancel the delete action 
+ Automatically close the confirmation modal 

- tabing the "Delete" button:
+ Perform the delete action
+ If success: Open a modal confirming that the delete action is done (CMMSG14), refresh page (if deleting items in a list page)/return to the previous screen (if deleting item from the details page)
+ If fail: CMMSG22
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. 「削除」をクリックすると確認モーダルが開きます

2. 削除確認モーダル
- 「X」ボタンまたはキャンセルボタンをクリックした場合：
+ 削除操作を直ちにキャンセルします
+ 確認モーダルを自動的に閉じます

- 「削除」ボタンをクリックした場合：
+ 削除操作を実行します
+ 成功した場合：削除操作が完了したことを確認するモーダルを開きます (CMMSG14)、ページを更新します (リストページでアイテムを削除している場合) / 前の画面に戻ります (詳細ページからアイテムを削除している場合)
+ 失敗した場合：CMMSG22
```

---

## CMR26: Cancel button

### 📝 Common Rule Detail (English / General)
```text
1. Purpose: exit the current workflow or form without saving changes, and return to the previous screen 

2. Upon tabed:
 - Stop all unsaved actions or input
 - Navigate back to the previous screen
 
3. Open a confirmation modal if unsaved changes exist
CMMSG28

- tabing the "X" button or the NO button:
+ Immediately cancel the action
+ Automatically close the confirmation modal 
+ Stay on the same screen 
+ All unsaved input information remains

- tabing the YES button:
+ Perform the action
+ Automatically close the confirmation modal 
+ Discard any data entered
+ Return to the previous screen
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
1. 目的: 変更を保存せずに現在のワークフローまたはフォームを終了し、前の画面に戻ります。

2. クリック時:
- 未保存のアクションまたは入力をすべて中止します。
- 前の画面に戻ります。

3. 未保存の変更がある場合は確認モーダルを開きます。
CMMSG28

- 「X」ボタンまたは「いいえ」ボタンをクリックした場合:
+ アクションを直ちにキャンセルします。
+ 確認モーダルを自動的に閉じます。
+ 同じ画面に留まります。
+ 未保存の入力情報はすべて保持されます。

- 「はい」ボタンをクリックした場合:
+ アクションを実行します。
+ 確認モーダルを自動的に閉じます。
+ 入力されたデータを破棄します。
+ 前の画面に戻ります。
```

---

## CMR27: Toggle ON/OFF

### 📝 Common Rule Detail (English / General)
```text
- Visual: slide left = OFF, slide right = ON
- Entire row (label + switch) should be tappable
- Toggle updates immediately on tap, with smooth animation
- Store toggle state locally and/or sync to backend as needed
```

---

## CMR28: Image Action

### 📝 Common Rule Detail (English / General)
```text
-When the user taps the image or icon, a bottom modal (Bottom modal CMR39) appears with the following options:

+View current profile image (if not default)
+Choose a new image from library
+Take a new photo with camera (Mobile only)
+Delete current image

**Depending on the specifications of each screen, the buttons that can be shown or hidden will be described individually for each screen.
```

---

## CMR29: Time picker

### 📝 Common Rule Detail (English / General)
```text
Initial Display
-No selection: Show place holder
-time selected: Display the selected time in format HH:MM:SS or HH:MM
-With default value: If a default time is pre-filled, display it accordingly

User Interaction Behavior
-Tap or tab on field: Open time picker
-Time selected: Selected time is shown in the field, picker closes
-Clear time: A clear (“✕”) button is displayed to reset the field
-Cancel without selecting: Keep previous value or leave empty

Calendar UI
-Use native picker for mobile, custom calendar UI for web
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
初期表示
-選択なし：プレースホルダーを表示
-時刻選択：選択された時刻をHH:MM:SSまたはHH:MMの形式で表示
-デフォルト値あり：デフォルトの時刻があらかじめ入力されている場合は、それに従って表示

ユーザーインタラクションの動作
-フィールドをタップまたはクリック：時刻ピッカーを開く
-時刻選択：選択された時刻がフィールドに表示され、ピッカーが閉じる
-時刻をクリア：クリアボタン（「✕」）が表示され、フィールドがリセットされる
-選択せずにキャンセル：前の値を保持するか、空白のままにする

カレンダーUI
-モバイルではネイティブピッカー、WebではカスタムカレンダーUIを使用する
```

---

## CMR30: Mail Validation

### 📝 Common Rule Detail (English / General)
```text
-Phải chứa đúng một ký tự @
-Phần trước @:
   +Chỉ chứa chữ cái, số, dấu chấm ., dấu gạch dưới _, hoặc dấu gạch ngang -.
   +Không để trống.

-Phần sau @ (tên miền):
   +Chỉ chứa chữ cái, số, dấu chấm ., hoặc dấu gạch ngang -.
   +Không để trống.

-Không chứa khoảng trắng ở bất kỳ vị trí nào.
-Tổng độ dài tối đa: 254 ký tự.

Nếu sai định dạng, báo lỗi MSG12
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
メールフィールドの要件：
最大254文字まで入力できます。
ローカルパートとドメインパートには、有効な文字（文字、数字、および._%+-などの特定の特殊文字）が含まれている必要があります。
一般的な形式は、ローカルパート@ドメインパートの構造に準拠する必要があります。
```

---

## CMR31: User ID Validation  (admin system)

### 📝 Common Rule Detail (English / General)
```text
ID Input Requirements:

Allows letters and numbers only, with a reasonable length between 6 and 15 characters.
Accepts both uppercase and lowercase letters, as well as digits.
Allow any special characters such as hyphens (-), underscores (_), etc.
The user ID must be unique and difficult to guess, avoiding simple IDs like "admin123".
Spaces are not allowed.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
ID 入力要件:

文字と数字のみで、6～15 文字の適切な長さで入力してください。
大文字、小文字、数字が使用できます。
ハイフン (-)、アンダースコア (_) などの特殊文字も使用できます。
ユーザー ID は一意で推測されにくいものにする必要があります。「admin123」のような単純な ID は避けてください。
スペースは使用できません。
```

---

## CMR32: Password Field Requirements (admin system)

### 📝 Common Rule Detail (English / General)
```text
Password Field Requirements:
Must be a minimum of 8 characters long.
Allows the input of the following character types:
1.Lowercase letters (a-z)
2.Uppercase letters (A-Z)
3.Digits (0-9)
4.Special characters (e.g., !@#$%^&*()_+=-)
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
パスワードフィールドの要件：
8文字以上である必要があります。
以下の文字種を入力できます。
1. 英小文字 (a-z)
2. 英大文字 (A-Z)
3. 数字 (0-9)
4. 特殊文字 (例: !@#$%^&*()_+=-)
```

---

## CMR33: Image Display

### 📝 Common Rule Detail (English / General)
```text
-Display a default image if the user hasn't uploaded one. The default image may vary, so it will be specified on each specification
-Display the correct uploaded image without distortion.
-tabing on the image will open a modal to preview it.K001 . If another action is available, it will be described on the screen and take priority.
-Show a loading spinner while the full-size image loads
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- ユーザーがアップロードしていない場合は、デフォルトの画像を表示します。(仕様書に記載します。)
- アップロードされた正しい画像を歪みなく表示します。
- 画像をクリックすると、モーダルビューが開き、画像をプレビューします。
Figma
```

---

## CMR34: Password Field Requirements

### 📝 Common Rule Detail (English / General)
```text
Password Field Requirements:

Must be a minimum of 8 characters long.
Allows the input of the following character types:
1.Lowercase letters (a-z)
2.Uppercase letters (A-Z)
3.Digits (0-9)
4.Special characters (e.g., !@#$%^&*()_+=-)
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
パスワードフィールドの要件：

8文字以上である必要があります。
以下の文字種を入力できます。
1. 英小文字 (a～z)
2. 英大文字 (A～Z)
3. 数字 (0～9)
4. 特殊文字 (例: !@#$%^&*()_+=-)
```

---

## CMR35: Breadcrumb Navigation

### 📝 Common Rule Detail (English / General)
```text
Show users their current page location and allow navigation to previous levels.

Structure: Horizontal list, separated by [>]

tabable: All items are tabable except the last one.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
目的：現在位置の表示と、上位ページへのナビゲーション。

構造：横並び、「> 」で区切り

クリック可能：最後以外の項目はリンク（クリック可能）
```

---

## CMR36: Sort Filter (Ascending / Descending) (Single field)

### 📝 Common Rule Detail (English / General)
```text
Allows the user to sort the displayed data in ascending or descending order.

Element：
Sort Toggle: Ascending (A→Z, oldest→newest) or Descending (Z→A, newest→oldest)

Behavior:
1. Default: No sorting is applied (data is displayed in original order).
2. First tab on the sort toggle: Sorting is applied in ascending order.
3. Subsequent tabs: The sorting state toggles between ascending and descending with each tab.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
表示されているデータを昇順または降順で並び替えることができます。

要素：
ソートトグル：昇順（A→Z、古い → 新しい）または降順（Z→A、新しい → 古い）

動作仕様：
1. デフォルト：ソートは適用されず、データは元の順序で表示される。
2. 最初のクリック：昇順でソートされる。
3. それ以降のクリック：クリックするたびに、昇順と降順が交互に切り替わる。
```

---

## CMR37: Checkbox Group

### 📝 Common Rule Detail (English / General)
```text
Allow users to:
- Select multiple items from a list. Each option is independent — users can check/uncheck any number of items.
- Select or deselect all items using a “Check All” checkbox.
- Delete selected items via a menu.

Structure:
- The first checkbox is “Check All”
- A vertical three-dot icon next to the first checkbox to opens a menu (dropdown).
Menu option: “Delete selected rows”　(button)

Functional Requirements
1. Check All Logic
- When "Check All" is selected, all individual checkboxes are checked.
- When any item is unchecked, "Check All" becomes unchecked

2. Delete Menu
- tabing the three-dot icon shows a dropdown with the option: "Delete selected items"
- If no item is selected, this option is disabled.
- tabing "Delete selected items" triggers:
(1)Delete success mesage CMMSG14:「 正常に削除されました。」
(2)Deletion of the selected rows

Example layout
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
ユーザーができること：
・リストから複数の項目を選択可能。各オプションは独立しており、ユーザーは任意の数をチェック／解除できる。
・「全選択」チェックボックスを使用して、すべての項目を一括で選択または解除できる。
・メニューを通じて、選択された項目を削除できる。

構築：
・最初のチェックボックスは「全選択」用。
・最初のチェックボックスの横に縦三点アイコン（︙）を表示し、これをクリックするとメニュー（ドロップダウン）が開く。メニュー項目: 「選択した行を削除」（ボタン）

機能要件:
1. チェック全選択のロジック：
・「全選択」が選択されると、すべての個別チェックボックスがチェックされる。
・いずれかの項目が未選択になると、「全選択」も未選択状態になる。

2. 削除メニュー：
・縦三点アイコンをクリックすると、「選択した行を削除」というオプションを含むドロップダウンメニューが表示される。
・何も選択されていない場合、このオプションは無効化（disabled)される。
・「選択した行を削除」をクリックすると、以下が実行される：
(1) 削除成功メッセージ「正常に削除されました。」を表示する：
(2) 選択された行を削除する

レイアウト例
```

---

## CMR38: Date Range Filter

### 📝 Common Rule Detail (English / General)
```text
Allows users to filter the list based on a selected date range, using a start date and an end date.

Trigger:
tabing the filter icon opens a date range picker (popup).

UI Elements
1. Filter Icon: Opens the calendar popup.
2. Start Date Picker: Calendar to select the start of the date range.
3. End Date Picker: Calendar to select the end of the date range.
4. Clear Button: Only appears after a date is selected. Used to reset the filter.

Behavior:
1. Initial State:
- No date range is applied.
- Popup only shows two calendars.
- Clear button is hidden.

2. User Interaction:
- User selects a Start Date and/or End Date.
- Once any date is selected, the Clear button appears below the calendars.
- The list is automatically filtered in real-time.
- If only Start Date is selected, filter from that date to the present.
- If only End Date is selected, filter from the beginning to that date.
- If both are selected, filter within the full range.

3. Clear Action:
- tabing Clear button resets both calendars and removes the filter.
- The list returns to showing all items.
- Clear button disappears again.

Validation Rules
- End Date must not be earlier than Start Date.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
ユーザーは開始日と終了日を使用して、指定した日付範囲に基づいてリストをフィルタリングできます。

トリガー：
フィルターアイコンをクリックすると、日付範囲ピッカー（ポップアップ）が表示されます。

UI要素
1. フィルターアイコン：カレンダーポップアップを開く。
2. 開始日ピッカー：日付範囲の開始日を選択するカレンダー。
3. 終了日ピッカー：日付範囲の終了日を選択するカレンダー。
4. クリアボタン：日付が選択された後にのみ表示され、フィルターをリセットするために使用される。

動作仕様：
1. 初期状態：
・日付範囲フィルターは適用されていない。
・ポップアップには2つのカレンダーのみが表示される。クリアボタンは非表示。

2. ユーザー操作：
・ユーザーは開始日および／または終了日を選択する。
・日付が1つでも選択されると、カレンダーの下にクリアボタンが表示される。
・リストはリアルタイムで自動的にフィルタリングされる。
・開始日のみ選択された場合、その日から現在までをフィルター。
・終了日ののみ選択された場合、最初からその日までをフィルター。
・両方が選択された場合、選択された範囲内でフィルター。

3. クリア動作：
・クリアボタンをクリックすると、両方のカレンダーがリセットされ、フィルターが解除される。
・リストはすべての項目を再表示する。
・クリアボタンは再び非表示になる。

検証ルール：終了日は開始日より前であってはならない。
```

---

## CMR39: Bottom Modal

### 📝 Common Rule Detail (English / General)
```text
-When the user taps a target UI element, bottom modal will slides up from the bottom of the screen.
-Semi-transparent black overlay
-Tap outside or pull down will close modal
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
-対象のUI要素をタップした時, 画面下部から表示されます。
-半透明のブラックオーバーレイ
-外側をタップするか、下にスワイプするとモーダルが閉じます
```

---

## CMR40: Tab Navigation

### 📝 Common Rule Detail (English / General)
```text
Tabs switch between different content or list groups. 
Switching can be done by tapping or swiping (on mobile). 
The active tab is highlighted.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
タブは異なるコンテンツやリストグループを切り替えるために使用されます。
モバイルではタップまたはスワイプで切り替え可能。
アクティブなタブは強調表示されます。
```

---

## CMR41: Required fields validation

### 📝 Common Rule Detail (English / General)
```text
Backend check:

Validate all required fields to ensure they are not empty or missing.

If any required field is missing or empty → return an error CMMSG29 with detailed info about which field failed and error message.

Frontend error display:

Receive error response from backend.

Show error message near the corresponding input field so the user can easily identify and correct the issue.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
バックエンド検証:

必須フィールドが空または欠落していないかチェック。

不足があれば、どのフィールドに問題があるか詳細を含むエラー CMMSG29 を返す。

フロントエンドエラー表示:

バックエンドのエラー応答を受け取り、

対応する入力フィールドの近くにエラーメッセージを表示し、ユーザーが修正しやすいようにする。
```

---

## CMR42: Email Format Validation

### 📝 Common Rule Detail (English / General)
```text
Backend check:
Total email length must not exceed 254 characters.
Local part (before @): max 64 characters.
Domain part (after @): max 255 characters, each label ≤ 63 characters.
Allowed characters:
 +Letters: a–z, A–Z
 +Numbers: 0–9
 +Special characters allowed in the local part: . _ % + -
 +Must not start/end with . or contain consecutive dots (..).

If invalid→ return an error MSG12 

Frontend error display:

Receive error response from backend.
Show error message near the corresponding input field so the user can easily identify and correct the issue.

Trùng với CMR30 nên sẽ thống nhất sử dụng CMR30 nhé
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
バックエンド検証:
メールアドレス全体の長さは 254文字以内。
ローカル部分（@の前）：最大 64文字。
ドメイン部分（@の後）：最大 255文字、各ラベルは最大 63文字。
使用可能な文字：
 +英字：a–z、A–Z
 +数字：0–9
 +ローカル部分で使用可能な記号：. _ % + -
 +. で開始・終了しないこと、.. の連続使用は禁止。

無効であれば、エラー MSG12 を返す。

フロントエンドエラー表示:

バックエンドのエラー応答を受け取り、
対応する入力フィールドの近くにエラーメッセージを表示し、ユーザーが修正しやすいようにする。
```

---

## CMR43: Password Validation Rule

### 📝 Common Rule Detail (English / General)
```text
Backend check:
Length: Minimum 8 characters
Must include:
  +At least one uppercase letter (A–Z)
  +At least one lowercase letter (a–z)
  +At least one digit (0–9)
  +At least one special character from the following set:  ! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ { | } ~

If invalid→ return an error MSG13 

Frontend error display:
Receive error response from backend.
Show error message near the corresponding input field so the user can easily identify and correct the issue.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
バックエンド検証:
メールアドレス全体の長さは 254文字以内。
ローカル部分（@の前）：最大 64文字。
ドメイン部分（@の後）：最大 255文字、各ラベルは最大 63文字。
使用可能な文字：
 +英字：a–z、A–Z
 +数字：0–9
 +ローカル部分で使用可能な記号：. _ % + -
 +. で開始・終了しないこと、.. の連続使用は禁止。

無効であれば、エラー MSG13 を返す。

フロントエンドエラー表示:

バックエンドのエラー応答を受け取り、
対応する入力フィールドの近くにエラーメッセージを表示し、ユーザーが修正しやすいようにする。
```

---

## CMR44: Modal bottom

### 📝 Common Rule Detail (English / General)
```text
Features
- Appears as a modal overlay on top of the current screen.
- Can be closed easily to return to the previous screen state.

User Interaction
- Swipe down (on mobile) or tap [X] to close the modal
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
主な特徴
- 現在の画面の上にモーダルオーバーレイとして表示される
- 簡単に閉じることができる（前の画面状態に戻れる）

ユーザーの操作
- モバイルではスワイプダウン、または [X] をタップしてモーダルを閉じる
```

---

## CMR45: 毎日配信       DAILY

### 📝 Common Rule Detail (English / General)
```text
- Danh hiệu dành cho user khi user đó có lượt livestream đạt được điều kiện logic sau:
+ phát livestream ít nhất 1 giờ liên tục trong 7 ngày liên tiếp
+ 1 ngày được tính bắt đầu từ 5:00 AM đến 4:59 hôm sau.
+ Nếu 1 phiên livestream bắt đầu trước 5:00 sáng và đi qua 5:00 sáng. nó vẫn chỉ được tính 1 là phiên và có start time bằng giờ start thực tế
-tất cả các loại livestream đều được count vào logic này
- phải đủ hàng ngày mới keep đc danh hiệu
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- 毎日配信しているユーザーを表彰するため
+ 7日間連続で、1セッションあたり15分以上の配信 一回の配信中時間が「1時間」以上（合計時間ではない）
+ 1日とは、午前3時から翌日の午前2時59分まで、午前5時から翌日の午前4時59分までとカウントされます。
+ 配信が午前3時午前5時をまたぐ場合は、その前の日の配信開始時刻としてカウントされます
+ Live2shotはカウントされません
+ 2ショットモード配信、ファミリア限定配信もカウントする。
```

---

## CMR47: Post Image

---

## CMR48: Text Overflow Handling

### 📝 Common Rule Detail (English / General)
```text
If the length of the text exceeds the display area limit, the excess part will automatically be hidden and replaced with “...”.
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
- テキストの長さが表示枠の制限を超えた場合、自動的に超過部分を非表示にし、「...」を表示するようになっています。
```

---

## CMR49: Create Draft button

### 📝 Common Rule Detail (English / General)
```text
Allows users to save a notification or form as a draft without publishing it. 

State: Enabled when required fields are filled; disabled otherwise. 

On tab (valid): Saves form data with status = "draft", publish_date = null. 
On tab (invalid): Shows validation errors or keeps button disabled. 
Success message: "Draft saved successfully.". Refer CMMSG50
Error message: "Failed to save draft. Please try again".  Refer CMMSG51
```

### 🇯🇵 Common Rule Detail (Japanese)
```text
下書きを作成ボタン。

目的：通知やフォームを公開せずに下書きとして保存できるようにする。

クリック時（有効なフォーム）：ステータス「draft」、公開日「null」でデータを保存。
クリック時（無効なフォーム）：バリデーションエラーを表示、またはボタンを無効のままにする。
成功メッセージ：「下書きが保存されました。」。参照：CMMSG50
エラーメッセージ：「下書きの保存に失敗しました。もう一度お試しください。」。参照：CMMSG51
```

---

## CMR50: N/A

### 📝 Common Rule Detail (English / General)
```text
CMR50
```

---

## CMR51: Tab các list live stream

### 📝 Common Rule Detail (English / General)
```text
- Hiển thị một tab riêng để người dùng có thể xem danh sách tất cả các livestream đang diễn ra (tuỳ theo các tiêu chí của từng tab)
- Thứ tự hiển thị: Sắp xếp theo thời gian bắt đầu livestream, theo thứ tự mới → cũ.
- Paging Infinite Scroll CMR01
- Pull to refresh: 
 + Vuốt từ mép trên cùng của danh sách livestream xuống 
 + Hiển thị biểu tượng loading vòng xoay ⟳ 
- Nếu không có live stream nào đang diễn ra (hoặc list following trống): hiện place holder 配信中のユーザーはございません。và list reccomendation các live stream - DESIGN UPDATE NEEDED
```

---

## CMR52: Block User

### 📝 Common Rule Detail (English / General)
```text
Rfer CMR52
```

---

## CMR53: Profile Livestream Section

### 📝 Common Rule Detail (English / General)
```text
Rfer  CMR53
```

---

## CMR54: Livestream Room Header - Viewer - Livestream Standby

### 📝 Common Rule Detail (English / General)
```text
Rfer CMR54
```

---

## CMR55: Livestream Room Header - Viewer - Livestream Normal

### 📝 Common Rule Detail (English / General)
```text
Rfer CMR55
```

---

## CMR56: Livestream Room Header - Viewer - Livestream 2shot

---

## CMR57: Livestream Room Header - Streamer - Livestream Normal

---

## CMR58: Livestream Room Header - Streamer - Livestream 2shot

---

## CMR59: Livestream Room Header - Streamer - Livestream Familia

---

## CMR60: Profile - thống kê số lượt follow

### 📝 Common Rule Detail (English / General)
```text
Rfer CMR60
```

---

## CMR61: Number Count

### 📝 Common Rule Detail (English / General)
```text
JP:
- 0 ~9999: hiện số bình thường
- 10,000 ~ : {số hàng chục nghìn}万

VD: 10,000 -> 1万   20,000 -> 2万   100,000 -> 10万 1,000,000 -> 100万
VD: 13,000 -> 1.3万   23,000 -> 2.3万   103,000 -> 10.3万 1,003,000 -> 100.3万

VD lẻ: 10,323 -> 1.03万 / 10,023 -> 1万 (làm tròn) / 10,923 -> 1.09万

VD lẻ: 108,134 -> 10.8万 / 100,134 -> 10万

VD lẻ: 1,003,123 -> 100.3万 / 1,000,123 -> 100万 / 1,030,123 -> 103万 / 1,000,923 -> 100万 


EN:
- 0 ~9999: hiện số bình thường
- 10,000 ~ : {số hàng nghìn}K
VD: 10,000 -> 10K   20,000 -> 20K   100,000 -> 100K
VD lẻ: 10,323 -> 10.3K / 10,023 -> 10K / 10,923 -> 10.9K
VD lẻ: 108,134 -> 108K / 100,134 -> 100.1K
VD lẻ: 1,003,123 -> 1000.1K / 1,000,923 -> 1000.9K
```

---

## CMR62: Deleted Account

### 📝 Common Rule Detail (English / General)
```text
Hiển thị text "đã bị xoá"
```

### 🔗 Metadata & Technical Details
- **Value JP:** `退会済みユーザー`
- **Value EN:** `Account Closed`

---

## CMR63: Picture in Picture

### 📝 Common Rule Detail (English / General)
```text
Refer CMR63
```

---

## CMR64: Streamer Update Stream Title

### 📝 Common Rule Detail (English / General)
```text
CMR64
```

---

## CMR65: Ngắt kết nối Livestream

### 📝 Common Rule Detail (English / General)
```text
CMR65
```

---

## CMR66: Upload file (avatar, cover image...video)

### 📝 Common Rule Detail (English / General)
```text
CMR66
```

---

## CMR67: Familia Class

### 📝 Common Rule Detail (English / General)
```text
CMR67
```

---

## CMR68: N/A

---
