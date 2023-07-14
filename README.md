# Csharp - JSON Tutorial
- A simple guide on handling JSON objects, mainly written for Unity
- 為 Unity 使用所撰寫的 JSON 轉換指南

## Requirement 必要條件
- **Install/安裝 Newtonsoft.Json**
> `Window` -> `Package Manager` -> `+` -> `Add package by name` -> `com.unity.nuget.newtonsoft-json`

## Conversion Tutorial 轉換教學
1. Add `using Newtonsoft.Json;` to the script that handles the conversion
    - 在必要程式中加入 `using Newtonsoft.Json;`
2. In JSON format, when you see square brackets `[ ]`, think of **`Array`** or **`List`**; when you see curly brackets `{ }`, think of **`class`** or **`struct`**
    - 在 JSON 格式中，中括號代表陣列、大括號代表物件
3. All the primitive types, as well as containers of them, are supported *(**eg.** `bool`, `int[]`, `List<string>`, etc.)*
    - 基本的檔案格式皆可以轉換
4. For both `Serialization` & `Deserialization` *(basically export & import)*, you need to follow the JSON structure and define a `class` *(or `struct`)* first
    - 輸出和輸入都必須先按照 JSON 架構撰寫一個對應的 `class` 或 `struct`
5. You can also have nested structures. It's the **variable name** that should match the JSON fields. The `class` name does not matter. *(Just like you would write `public string name;` rather than `public name name;`)*
    - 你也可以有多層架構。注意是 **變數名稱** 需要和 JSON 中相同。 `class` 的名稱隨意。 *(就像你會寫 `public string name;` 而不是 `public name name;`)*
6. **Serialization:** Pass the object into `JsonConvert.SerializeObject()`, it will return a single-line `string` in JSON format. You can then use functions like `File.WriteAllText` to write it out.
    - **轉換成 JSON:** 將物件傳入 `JsonConvert.SerializeObject()`， 它便會回傳單行 JSON 格式的 `string`。可以再用`File.WriteAllText`等方式將其寫出。
7. **Deserialization:** You can first use functions like `File.ReadAllText` to obtain the JSON `string`, then pass it into `JsonConvert.DeserializeObject<>()` to convert to a more usable object.
    - **讀取 JSON:** 可以先用 `File.ReadAllText` 等方法讀取 JSON 的 `string`，接著傳入 `JsonConvert.DeserializeObject<>()` 來轉換成可用的物件。

> You need to pass in the defined `class` inside the `< >` so that it knows how to parse the `string`

> 必須將前述的 `class` 結構傳入 `< >` 來正確轉換

## Code Example 程式示範
```cs
    void Start()
    {
        Demo demo = new Demo
        {
            val = 1
        };

        File.WriteAllText(PATH, JsonConvert.SerializeObject(demo));
        Demo import = JsonConvert.DeserializeObject<Demo>(File.ReadAllText(PATH));
    }

    private struct Demo
    {
        public int val;
    }
```

## Structure Samples 架構範例

#### 1.
- **JSON**
```json
{
  "name": "John",
  "age": 69,
  "interests": [ "Piano", "Movie" ]
}
```

- **C#**
```cs
    private class HR
    {
        public string name;
        public int age;
        public string[] interests;
    }
```

#### 2.
- **JSON**
```json
{
  "ids": [ 1, 4, 8, 16 ],
  "deadline": "2023-07-14T00:00:00+08:00"
}
```

- **C#**
```cs
    private class ToDo
    {
        public List<int> ids;
        public DateTime deadline;
    }
```

#### 3.
> If a primitive field could be `null`, add `?` after the `type`

> 如果值可能是 `null`，在 `type` 的後面加上 `?`

- **JSON**
```json
{
  "player_name": "Kirby",
  "is_male": null,
  "player_stats": {
    "S": 8,
    "P": 7,
    "E": 5,
    "C": 6,
    "I": 4,
    "A": 2,
    "L": 3
  }
}
```

- **C#**
```cs
    private class Fallout
    {
        public string player_name;
        public bool? is_male;

        public SPECIAL player_stats;

        public class SPECIAL
        {
            public int S;
            public int P;
            public int E;
            public int C;
            public int I;
            public int A;
            public int L;
        }
    }
```

#### 4.
> The outermost layer is `[ ]`，thus use `JsonConvert.DeserializeObject<Clip[]>(json_string);` directly

> 最外層為 `[ ]`，故直接用 `JsonConvert.DeserializeObject<Clip[]>(json_string);`

- **JSON**
```json
[
  {
    "guid": "87564231",
    "start": 6.9
  },
  {
    "guid": "13246578",
    "start": 42.0
  }
]
```

- **C#**
```cs
    private class Clip
    {
        public string guid;
        public float start;
    }
```

#### 5.
- **JSON**
```json
{
  "days_played": 420,
  "obtained_skills": [
    {
      "name": "Fire Ball",
      "description": "Shoot a Ball of Fire",
      "damage": 6.9,
      "status_effect": null
    },
    {
      "name": "Omega Fire Ball",
      "description": "Fire Ball, but Omega",
      "damage": 69.0,
      "status_effect": {
        "DoT": 4.2,
        "duration": 42.0
      }
    }
  ]
}
```

- **C#**
```cs
    private struct RPG
    {
        public int days_played;

        public Skill[] obtained_skills;

        public struct Skill
        {
            public string name;

            public string description;

            public float damage;

            public Effect status_effect;

            public class Effect
            {
                public float DoT;
                public float duration;
            }
        }
    }
```

#### Bonus
> You can also omit unwanted field by simply not defining it in the `class`

> 你也可以透過不在 `class` 中宣告來忽略不用的值

- **JSON**
```json
{
  "values": [ 69.42, 420.69 ],
  "metadata": "TmV2ZXIgR29ubmEgR2l2ZSBZb3UgVXAsIE5ldmVyIEdvbm5hIExldCBZb3UgRG93bg=="
}
```

- **C#**
```cs
    private class Track
    { 
        public float[] values;
    }
```

# Documentation
- [Newtonsoft.Json GitHub](https://github.com/JamesNK/Newtonsoft.Json)
- [Newtonsoft.Json Help Page](https://www.newtonsoft.com/json/help)
