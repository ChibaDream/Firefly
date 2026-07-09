---
title: Markdown Mermaid 鍥捐〃
published: 1970-01-01
pinned: false
description: 涓€涓寘鍚� Mermaid 鐨� Markdown 鍗氬鏂囩珷绠€鍗曠ず渚嬨€�
tags: [Markdown, 鍗氬, Mermaid, Firefly]
category: 鏂囩珷绀轰緥
draft: false
---
## Markdown 涓� Mermaid 鍥捐〃瀹屾暣鎸囧崡

鏈枃婕旂ず濡備綍鍦� Markdown 鏂囨。涓娇鐢� Mermaid 鍒涘缓鍚勭澶嶆潅鍥捐〃锛屽寘鎷祦绋嬪浘銆佹椂搴忓浘銆佺敇鐗瑰浘銆佺被鍥惧拰鐘舵€佸浘銆�

## 娴佺▼鍥剧ず渚�

娴佺▼鍥鹃潪甯搁€傚悎琛ㄧず娴佺▼鎴栫畻娉曟楠ゃ€�




```mermaid
graph TD
    A[寮€濮媇 --> B{鏉′欢妫€鏌
    B -->|鏄瘄 C[澶勭悊姝ラ 1]
    B -->|鍚 D[澶勭悊姝ラ 2]
    C --> E[瀛愯繃绋媇
    D --> E
    subgraph E [瀛愯繃绋嬭鎯匽
        E1[瀛愭楠� 1] --> E2[瀛愭楠� 2]
        E2 --> E3[瀛愭楠� 3]
    end
    E --> F{鍙︿竴涓喅绛杴
    F -->|閫夐」 1| G[缁撴灉 1]
    F -->|閫夐」 2| H[缁撴灉 2]
    F -->|閫夐」 3| I[缁撴灉 3]
    G --> J[缁撴潫]
    H --> J
    I --> J
```

## 鏃跺簭鍥剧ず渚�

鏃跺簭鍥炬樉绀哄璞′箣闂撮殢鏃堕棿鐨勪氦浜掋€�

```mermaid
sequenceDiagram
    participant User as 鐢ㄦ埛
    participant WebApp as 缃戦〉搴旂敤
    participant Server as 鏈嶅姟鍣�
    participant Database as 鏁版嵁搴�

    User->>WebApp: 鎻愪氦鐧诲綍璇锋眰
    WebApp->>Server: 鍙戦€佽璇佽姹�
    Server->>Database: 鏌ヨ鐢ㄦ埛鍑嵁
    Database-->>Server: 杩斿洖鐢ㄦ埛鏁版嵁
    Server-->>WebApp: 杩斿洖璁よ瘉缁撴灉
    
    alt 璁よ瘉鎴愬姛
        WebApp->>User: 鏄剧ず娆㈣繋椤甸潰
        WebApp->>Server: 璇锋眰鐢ㄦ埛鏁版嵁
        Server->>Database: 鑾峰彇鐢ㄦ埛鍋忓ソ
        Database-->>Server: 杩斿洖鍋忓ソ璁剧疆
        Server-->>WebApp: 杩斿洖鐢ㄦ埛鏁版嵁
        WebApp->>User: 鍔犺浇涓€у寲鐣岄潰
    else 璁よ瘉澶辫触
        WebApp->>User: 鏄剧ず閿欒娑堟伅
        WebApp->>User: 鎻愮ず閲嶆柊杈撳叆
    end
```

## 鐢樼壒鍥剧ず渚�

鐢樼壒鍥鹃潪甯搁€傚悎鏄剧ず椤圭洰杩涘害鍜屾椂闂寸嚎銆�

```mermaid
gantt
    title 缃戠珯寮€鍙戦」鐩椂闂寸嚎
    dateFormat  YYYY-MM-DD
    axisFormat  %m/%d
    
    section 璁捐闃舵
    闇€姹傚垎鏋�      :a1, 2023-10-01, 7d
    UI璁捐                 :a2, after a1, 10d
    鍘熷瀷鍒涘缓        :a3, after a2, 5d
    
    section 寮€鍙戦樁娈�
    鍓嶇寮€鍙�      :b1, 2023-10-20, 15d
    鍚庣寮€鍙�       :b2, after a2, 18d
    鏁版嵁搴撹璁�           :b3, after a1, 12d
    
    section 娴嬭瘯闃舵
    鍗曞厓娴嬭瘯              :c1, after b1, 8d
    闆嗘垚娴嬭瘯       :c2, after b2, 10d
    鐢ㄦ埛楠屾敹娴嬭瘯   :c3, after c2, 7d
    
    section 閮ㄧ讲
    鐢熶骇鐜閮ㄧ讲     :d1, after c3, 3d
    鍙戝竷                    :milestone, after d1, 0d
```

## 绫诲浘绀轰緥

绫诲浘鏄剧ず绯荤粺鐨勯潤鎬佺粨鏋勶紝鍖呮嫭绫汇€佸睘鎬с€佹柟娉曞強鍏跺叧绯汇€�

```mermaid
classDiagram
    class User {
        +String username
        +String password
        +String email
        +Boolean active
        +login()
        +logout()
        +updateProfile()
    }
    
    class Article {
        +String title
        +String content
        +Date publishDate
        +Boolean published
        +publish()
        +edit()
        +delete()
    }
    
    class Comment {
        +String content
        +Date commentDate
        +addComment()
        +deleteComment()
    }
    
    class Category {
        +String name
        +String description
        +addArticle()
        +removeArticle()
    }
    
    User "1" -- "*" Article : 鍐欎綔
    User "1" -- "*" Comment : 鍙戣〃
    Article "1" -- "*" Comment : 鎷ユ湁
    Article "1" -- "*" Category : 灞炰簬
```

## 鐘舵€佸浘绀轰緥

鐘舵€佸浘鏄剧ず瀵硅薄鍦ㄥ叾鐢熷懡鍛ㄦ湡涓粡鍘嗙殑鐘舵€佸簭鍒椼€�

```mermaid
stateDiagram-v2
    [*] --> 鑽夌ǹ
    
    鑽夌ǹ --> 瀹℃牳涓� : 鎻愪氦
    瀹℃牳涓� --> 鑽夌ǹ : 鎷掔粷
    瀹℃牳涓� --> 宸叉壒鍑� : 鎵瑰噯
    宸叉壒鍑� --> 宸插彂甯� : 鍙戝竷
    宸插彂甯� --> 宸插綊妗� : 褰掓。
    宸插彂甯� --> 鑽夌ǹ : 鎾ゅ洖
    
    state 宸插彂甯� {
        [*] --> 娲昏穬
        娲昏穬 --> 闅愯棌 : 涓存椂闅愯棌
        闅愯棌 --> 娲昏穬 : 鎭㈠
        娲昏穬 --> [*]
        闅愯棌 --> [*]
    }
    
    宸插綊妗� --> [*]
```

## 楗煎浘绀轰緥

楗煎浘闈炲父閫傚悎鏄剧ず姣斾緥鍜岀櫨鍒嗘瘮鏁版嵁銆�

```mermaid
pie title 缃戠珯娴侀噺鏉ユ簮鍒嗘瀽
    "鎼滅储寮曟搸" : 45.6
    "鐩存帴璁块棶" : 30.1
    "绀句氦濯掍綋" : 15.3
    "鎺ㄨ崘閾炬帴" : 6.4
    "鍏朵粬鏉ユ簮" : 2.6
```

## 鎬荤粨

Mermaid 鏄湪 Markdown 鏂囨。涓垱寤哄悇绉嶇被鍨嬪浘琛ㄧ殑寮哄ぇ宸ュ叿銆傛湰鏂囨紨绀轰簡濡備綍浣跨敤娴佺▼鍥俱€佹椂搴忓浘銆佺敇鐗瑰浘銆佺被鍥俱€佺姸鎬佸浘鍜岄ゼ鍥俱€傝繖浜涘浘琛ㄥ彲浠ュ府鍔╂偍鏇存竻鏅板湴琛ㄨ揪澶嶆潅鐨勬蹇点€佹祦绋嬪拰鏁版嵁缁撴瀯銆�

瑕佷娇鐢� Mermaid锛屽彧闇€鍦ㄤ唬鐮佸潡涓寚瀹� mermaid 璇█锛屽苟浣跨敤绠€娲佺殑鏂囨湰璇硶鎻忚堪鍥捐〃銆侻ermaid 浼氳嚜鍔ㄥ皢杩欎簺鎻忚堪杞崲涓虹編瑙傜殑鍙鍖栧浘琛ㄣ€�

灏濊瘯鍦ㄦ偍鐨勪笅涓€绡囨妧鏈崥瀹㈡枃绔犳垨椤圭洰鏂囨。涓娇鐢� Mermaid 鍥捐〃 - 瀹冧滑灏嗕娇鎮ㄧ殑鍐呭鏇村姞涓撲笟涓旀洿鏄撶悊瑙ｏ紒