# RapidJson

- ### 파싱에 소요되는 시간, 사용하는 메모리 측면에서 높은 성능을 가진 C++ JSON 처리 라이브러리

    - #### GITHUB: https://github.com/Tencent/rapidjson.git

    - #### 공식 문서: https://rapidjson.org/md_doc_tutorial.html

<br>

- ### String 형태의 JSON 파싱

    ```c++
    int main() {
        const char* json = "{\"project\":\"rapidjson\",\"stars\":10}";

        Document d;
        d.Parse(json); // String 형태의 JSON 객체화

        printf("%s\n", d["project"].GetString()); // project 키의 String Value 출력
        return 0;
    }
    ```

    ```
    rapidjson
    ```

<br>

- ### JSON 객체 생성, 문자열화(stringify)

    ```c++
    int main() {
        Document d;
        d.SetObject(); // Document 를 Object로 초기화

        Value v(true);
        d.AddMember("bool_member", v, d.GetAllocator()); //메인 Object에 bool_member: true인 member 추가

        Value player_arr(kArrayType); // JSON Array 생성
        for (int i = 0; i < 8; i++) {
            Value curr_player_obj(kObjectType); // Array에 들어갈 객체 생성

            curr_player_obj.AddMember("index", Value(i), d.GetAllocator()); // 객체에 index: i인 member 추가

            char curr_player_name[256];
            sprintf_s(curr_player_name, "%c%c%c", 'a' + i, 'a' + i, 'a' + i); // AAA, BBB, CCC, ... 형태의 임의의 이름 생성
            Value name_val = Value();
            name_val.SetString(curr_player_name, strlen(curr_player_name), d.GetAllocator()); // Value에 문자열 할당
            curr_player_obj.AddMember("Name", name_val, d.GetAllocator()); // Name: 임의의 이름인 member 추가

            player_arr.PushBack(curr_player_obj, d.GetAllocator()); // JSON Array 끝에 현재 Player 객체 추가
        }
        d.AddMember("players", player_arr, d.GetAllocator()); // 메인 Object의 player에 JSON Array 할당

        StringBuffer out_buf;
        Writer<StringBuffer> writer(out_buf);
        d.Accept(writer); // Document 문자열화
        printf("%s\n", out_buf.GetString()); // 출력

        return 0;
    }
    ```

    ```
    {"bool_member":true,"players":[{"index":0,"Name":"aaa"},{"index":1,"Name":"bbb"},{"index":2,"Name":"ccc"},{"index":3,"Name":"ddd"},{"index":4,"Name":"eee"},{"index":5,"Name":"fff"},{"index":6,"Name":"ggg"},{"index":7,"Name":"hhh"}]}
    ```