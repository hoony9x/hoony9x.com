---
title: 2020 카카오 신입 개발자 블라인드 채용 지원 후기
author: hoony9x
date: 2019-10-16 21:00:00
image: assets/images/2019-10-16-2020-kakao-blind-recruitment/IMG_0223-1.jpeg
categories:
  - "Recruitment"
  - "2019"
tags:
  - "Recruitment"
  - "Interview"
  - "Kakao"
  - "Algorithm"
  - "Develop"
  - "PS"
featured: true
---

매년 그랬듯이, 이 채용 공지 역시 8월에 올라왔어요. 당장 내년 2월 졸업 예정자는 아니지만 어차피 4학년 2학기만 남아있는 상태고 곧 군필자가 되는 상태라 지원을 했어요.

일단 결론부터 말하면 떨어졌어요.

저는 PS 를 잘 하는 사람은 아니에요. 그래도 누군가에게 이 후기가 도움이 될 수도 있으니 한번 적어볼게요.

## 지원하기

채용 소식은 여러 경로를 통해 올라왔는데 공식 지원 링크는 여기였어요. 들어가보면 어느 계열사에 지원할 수 있는지, 그리고 대략적인 일정이 나와 있어요. 저는 카카오에 지원했어요.

![배너 이미지](/assets/images/2019-10-16-2020-kakao-blind-recruitment/8a20d83b-c976-4c0e-a4cd-778963703513-1.jpeg)

지원할 때 적는 정보는

- 이름
- 이메일
- 연락처
- 지원하는 회사 (계열사)

이게 전부에요. 생일이나 학교 정보 등등 추가로 적는 것이 없어요.

![지원서 접수 후 화면](/assets/images/2019-10-16-2020-kakao-blind-recruitment/Screen-Shot-2019-10-15-at-4.33.11-PM.png)

접수 완료 후 온 메일의 모습이에요. 이제 저 날짜에 온라인 코딩 테스트를 진행하면 됩니다.

## 온라인 코딩 테스트

![온라인 코딩 테스트 안내 메일](/assets/images/2019-10-16-2020-kakao-blind-recruitment/Screen-Shot-2019-10-15-at-9.24.38-PM.png)

전역하고 1주일 만에 코딩 테스트를 치르려고 하니까 정말 힘들었어요. 굳어버린 머리가 깨어나지 않은 상태라 코드 작성을 빠르게 하지 못했어요.

이 코딩 테스트의 공식 해설은 [여기](https://tech.kakao.com/2019/10/02/kakao-blind-recruitment-2020-round1/)에 들어가면 볼 수 있어요. 저는 1, 2, 3, 4번 문제를 풀었어요. 4번까지 풀고 나니 시간이 얼마 남지 않아서 더 풀지 않았어요.

테스트 플랫폼으로 [프로그래머스](https://programmers.co.kr/)를 이용했어요.

### 1번

보자마자 바로 코드 작성을 시작했어요. 그런데 자꾸 특정 테스트 케이스에서 WA 가 발생을 했어요. 왜 그런가 했더니 숫자가 10 이상이 될 때를 고려하지 않고 길이 계산을 했더라고요. 이걸 빠르게 발견하지 못해서 30분정도 삽질했어요.

#### 코딩 테스트 당시 작성했던 1번 문제 코드

```cpp
#include <bits/stdc++.h>
using namespace std;

int solution(string s) {
  int min_length = numeric_limits<int>::max();

  for(int len_itr = 1; len_itr <= (int)(s.size() / 2) + 1; len_itr++) {
    vector< pair<string, int> > C;

    for(int pos = 0; pos < (int)s.size(); pos += len_itr) {
      string sub_str = s.substr(pos, len_itr);

      if(C.empty()) {
        C.push_back(make_pair(sub_str, 1));
      }
      else {
        if(C.back().first == sub_str) {
          C.back().second += 1;
        }
        else {
          C.push_back(make_pair(sub_str, 1));
        }
      }
    }

    int sub_len = 0;
    for(auto &elem: C) {
      int substr_size = (int)elem.first.size();
      int cnt = elem.second;

      sub_len += substr_size;
      if(cnt > 1) {
        sub_len += (int)(to_string(cnt)).size();
      }
    }

    min_length = min(min_length, sub_len);
  }
  
  return min_length;
}
```

항상 저런 자잘한 조건 체크를 못해서 WA 를 내는 경우가 많았는데 이번에도 또 이랬어요(...)

### 2번

[BOJ 2504](https://www.acmicpc.net/problem/2504), [BOJ 3345](https://www.acmicpc.net/problem/3345), [BOJ 10422](https://www.acmicpc.net/problem/10422) 같은 문제를 풀어본 적이 있다면 매우 익숙한 문제에요. 저는 Stack 자료구조를 사용해서 풀었어요. 쌍이 맞지 않는 괄호 문자에 대해서 문제에서 하라는 지시사항대로 코드를 작성하면 쉽게 풀리는 문제였어요.

#### 코딩 테스트 당시 작성했던 2번 문제 코드

```cpp
#include <bits/stdc++.h>
using namespace std;

bool chkIfCorrect(string s) {
  stack<char> chk_s;
  for(auto &c: s) {
    if(chk_s.empty()) {
      chk_s.push(c);
    }
    else {
      if(c == '(') {
        chk_s.push(c);
      }
      else {
        if(chk_s.top() == '(') {
          chk_s.pop();
        }
        else {
          chk_s.push(c);
        }
      }
    }
  }
  
  return chk_s.empty();
};

string solution(string p) {
  if(p.empty()) {
    return "";
  }
  else {
    if(chkIfCorrect(p) == true) {
      return p;
    }
  }
  
  int left_cnt = 0;
  int right_cnt = 0;
  
  int pos = 0;
  for(pos = 0; pos < p.size(); pos++) {
    if(p[pos] == '(') {
      left_cnt += 1;
    }
    else {
      right_cnt += 1;
    }

    if(left_cnt == right_cnt) {
      break;
    }
  }
  
  string u = p.substr(0, pos + 1);
  string v = p.substr(pos + 1);

  string answer;
  if(chkIfCorrect(u) == true) {
    v = solution(v);
    answer = u + v;
  }
  else {
    string fix_s = "(";
    fix_s += solution(v);
    fix_s += ")";

    for(auto i = 1; i < u.size() - 1; i++) {
      if(u[i] == '(') {
        fix_s.push_back(')');
      }
      else {
        fix_s.push_back('(');
      }
    }

    answer = fix_s;
  }
  
  return answer;
}
```

이 문제는 별 문제 없이 통과했어요.

### 3번

문제를 보자마자 한숨이 나왔어요. 특정 알고리즘을 요구하는 문제는 아니지만 꽤나 귀찮은 문제가 되겠구나 싶었죠.
자물쇠를 돌려볼 것인가 아니면 키를 돌려볼 것인가 여부에 대해서 처음에 생각을 했는데 어차피 뭘 하나 똑같다는 것을 인지하는 데 오래 걸리지는 않았어요.

저는 자물쇠를 가운데에 두고 좌표평면을 확장시킨 후 열쇠를 이동시켜가며 맞는 경우가 있는지를 확인하도록 코드를 작성했어요. 어차피 N과 M의 값이 작아서 4중 for-loop 까지도 문제가 없을 것이라 판단했어요.

좌표평면은 가로세로 (2 * M + N) 의 사이즈를 가지며 이 평면의 중앙에 자물쇠를 놓았어요. 그리고 키를 왼쪽 위부터 오른쪽 아래로 한칸씩 이동시키며 맞는 경우가 있는지 확인했어요.

키를 90도 회전 시키는 부분이 또 문제였어요. 회전변환을 하려면 이 키의 좌표를 원점으로 옮긴 후 회전변환을 해야 하는데 키의 가로세로 길이가 짝수인 경우 원점으로 옮기기가 좀 골때렸어요. 혹시나 될까 하는 생각으로 double 자료형을 잠시 써서 회전을 해봤는데 다행히(?) 별 문제 없더라고요.

우여곡절 끝에 코드를 작성해서 문제를 풀었어요. 아래는 당시에 작성했던 코드에요.

#### 코딩 테스트 당시 작성했던 3번 문제 코드

```cpp
#include <bits/stdc++.h>
using namespace std;

bool solution(vector< vector<int> > key, vector< vector<int> > lock) {
  int M = (int)key.size();
  int N = (int)lock.size();
  
  vector< vector<int> > G(2 * M + N);
  for(auto &g: G) {
    g.resize(2 * M + N);
    fill(g.begin(), g.end(), 2);
  }
  
  int total_hole_cnt = 0;
  for(auto i = M; i < M + N; i++) {
    for(auto j = M; j < M + N; j++) {
      G[i][j] = !lock[i - M][j - M];
      if(G[i][j] == 1) {
        total_hole_cnt += 1;
      }
    }
  }
  
  auto key1 = key; // same key
  auto key2 = key; // 90도 회전
  auto key3 = key; // 180도 회전
  auto key4 = key; // 270도 회전
  
  if(M % 2 != 0) {
    for(auto i = 0; i < M; i++) {
      for(auto j = 0; j < M; j++) {
        int orig_X = j - (M / 2);
        int orig_Y = i - (M / 2);

        int conv_X = -orig_Y + (M / 2);
        int conv_Y = orig_X + (M / 2);

        key2[conv_Y][conv_X] = key[i][j];
      }
    }

    for(auto i = 0; i < M; i++) {
      for(auto j = 0; j < M; j++) {
        int orig_X = j - (M / 2);
        int orig_Y = i - (M / 2);

        int conv_X = orig_Y + (M / 2);
        int conv_Y = -orig_X + (M / 2);

        key4[conv_Y][conv_X] = key[i][j];
      }
    }
  }
  else {
    for(auto i = 0; i < M; i++) {
      for(auto j = 0; j < M; j++) {
        double orig_X = (double)j - (double)(M - 1) / 2.0;
        double orig_Y = (double)i - (double)(M - 1) / 2.0;

        int conv_X = (int)(-orig_Y + (double)(M - 1) / 2.0);
        int conv_Y = (int)(orig_X + (double)(M - 1) / 2.0);

        key2[conv_Y][conv_X] = key[i][j];
      }
    }

    for(auto i = 0; i < M; i++) {
      for(auto j = 0; j < M; j++) {
        double orig_X = (double)j - (double)(M - 1) / 2.0;
        double orig_Y = (double)i - (double)(M - 1) / 2.0;

        int conv_X = (int)(orig_Y + (double)(M - 1) / 2.0);
        int conv_Y = (int)(-orig_X + (double)(M - 1) / 2.0);

        key4[conv_Y][conv_X] = key[i][j];
      }
    }
  }
  
  for(auto i = 0; i < M; i++) {
    for(auto j = 0; j < M; j++) {
      key3[i][j] = key[M - 1 - i][M - 1 - j];
    }
  }
  
  for(auto i = 0; i < M + N; i++) {
    for(auto j = 0; j < M + N; j++) {
      int cnt = 0;

      for(auto y = i; y < i + M; y++) {
        for(auto x = j; x < j + M; x++) {

          if(G[y][x] == 1 && key1[y - i][x - j] == 1) {
            cnt += 1;
          }
          else if(G[y][x] == 0 && key1[y - i][x - j] != 0) {
            cnt -= 1;
          }
        }
      }

      if(total_hole_cnt == cnt) {
        return true;
      }
    }
  }
  
  for(auto i = 0; i < M + N; i++) {
    for(auto j = 0; j < M + N; j++) {
      int cnt = 0;

      for(auto y = i; y < i + M; y++) {
        for(auto x = j; x < j + M; x++) {

          if(G[y][x] == 1 && key2[y - i][x - j] == 1) {
            cnt += 1;
          }
          else if(G[y][x] == 0 && key2[y - i][x - j] != 0) {
            cnt -= 1;
          }
        }
      }

      if(total_hole_cnt == cnt) {
        return true;
      }
    }
  }
  
  for(auto i = 0; i < M + N; i++) {
    for(auto j = 0; j < M + N; j++) {
      int cnt = 0;

      for(auto y = i; y < i + M; y++) {
        for(auto x = j; x < j + M; x++) {

          if(G[y][x] == 1 && key3[y - i][x - j] == 1) {
            cnt += 1;
          }
          else if(G[y][x] == 0 && key3[y - i][x - j] != 0) {
            cnt -= 1;
          }
        }
      }

      if(total_hole_cnt == cnt) {
        return true;
      }
    }
  }
  
  for(auto i = 0; i < M + N; i++) {
    for(auto j = 0; j < M + N; j++) {
      int cnt = 0;

      for(auto y = i; y < i + M; y++) {
        for(auto x = j; x < j + M; x++) {

          if(G[y][x] == 1 && key4[y - i][x - j] == 1) {
            cnt += 1;
          }
          else if(G[y][x] == 0 && key4[y - i][x - j] != 0) {
            cnt -= 1;
          }
        }
      }

      if(total_hole_cnt == cnt) {
        return true;
      }
    }
  }
  
  return false;
}
```

너무 막 짠 코드라 저가 봐도 맘에 들지 않아요.

### 4번

이거도 어디서 한번 봤던 유형인데 이런 유형을 직접 풀어본 적이 없었어요. 입력 데이터의 크기를 보니 [특정 알고리즘(트라이)](https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4)이 요구된다는 것은 바로 알았지만 그 알고리즘이 뭔지 생각이 안나는 거에요. (그건 또 당연한게, 풀어 본 적이 없으니...) 그런데 생각이 나는 대로 코드를 작성해 봤는데 트라이 알고리즘 비스무리하게 푼거 같아요.

문제의 입력 데이터가 "효율성" 부분과 "정확성" 부분으로 나뉘어 있는데, 큰 사이즈의 입력은 효율성 부분에서 들어와요. 그런데 "정확성" 부분에서 '?' 로만 이루어진 단어가 테스트 케이스로 존재하지 않았던 것 같아요. (이 데이터가 "효율성" 입력에 존재했던 것으로 보여요)

#### 코딩 테스트 당시 작성했던 4번 문제 코드

```cpp
#include <bits/stdc++.h>
using namespace std;

class Node {
public:
  int cnt;
  map<char, Node*> next;
};

void forwardInsert(map<char, Node*> &f_dict, string &word, int pos) {
  if(pos >= (int)word.size()) {
    return;
  }
  
  if(f_dict.find(word[pos]) != f_dict.end()) {
    auto &data = f_dict[word[pos]];
    data->cnt += 1;
    forwardInsert(data->next, word, pos + 1);
  }
  else {
    auto n_data = new Node;
    n_data->cnt = 1;
    f_dict[word[pos]] = n_data;

    forwardInsert(n_data->next, word, pos + 1);
  }
}

void backwardInsert(map<char, Node*> &b_dict, string &word, int pos) {
  if(pos < 0) {
    return;
  }
  
  if(b_dict.find(word[pos]) != b_dict.end()) {
    auto &data = b_dict[word[pos]];
    data->cnt += 1;
    backwardInsert(data->next, word, pos - 1);
  }
  else {
    auto n_data = new Node;
    n_data->cnt = 1;
    b_dict[word[pos]] = n_data;

    backwardInsert(n_data->next, word, pos - 1);
  }
}

int forwardSearch(map<char, Node*> &f_dict, string &query, int pos) {
  char cur_c = query[pos];
  
  if(cur_c == '?') {
    return -1;
  }
  
  if(f_dict.find(cur_c) == f_dict.end()) {
    return 0;
  }
  else {
    int cnt = f_dict[cur_c]->cnt;
    auto &next_dict = f_dict[cur_c]->next;

    int answer = forwardSearch(next_dict, query, pos + 1);
    if(answer == -1) {
      answer = cnt;
    }

    return answer;
  }
}

int backwardSearch(map<char, Node*> &b_dict, string &query, int pos) {
  char cur_c = query[pos];
  
  if(cur_c == '?') {
    return -1;
  }
  
  if(b_dict.find(cur_c) == b_dict.end()) {
    return 0;
  }
  else {
    int cnt = b_dict[cur_c]->cnt;
    auto &next_dict = b_dict[cur_c]->next;

    int answer = backwardSearch(next_dict, query, pos - 1);
    if(answer == -1) {
      answer = cnt;
    }

    return answer;
  }
}

vector<int> solution(vector<string> words, vector<string> queries) {
  map<int, map<char, Node*> > forward_chk;
  map<int, map<char, Node*> > backward_chk;
  
  for(auto &word: words) {
    int word_size = (int)word.size();

    if(forward_chk.find(word_size) == forward_chk.end()) {
      map<char, Node*> n_dictionary;
      forward_chk[word_size] = n_dictionary;
    }

    forwardInsert(forward_chk[word_size], word, 0);

    if(backward_chk.find(word_size) == backward_chk.end()) {
      map<char, Node*> n_dictionary;
      backward_chk[word_size] = n_dictionary;
    }

    backwardInsert(backward_chk[word_size], word, (int)word.size() - 1);
  }
  
  vector<int> answer; answer.reserve(100000);
  
  for(auto &query: queries) {
    int query_size = (int)query.size();

    /* special case */
    if(query.back() == '?' && query.front() == '?') {
      int total_cnt = 0;
      for(auto &elem: forward_chk[query_size]) {
        total_cnt += elem.second->cnt;
      }

      answer.push_back(total_cnt);

      continue;
    }

    if(query.back() == '?') { // forward search
      if(forward_chk.find(query_size) == forward_chk.end()) {
        answer.push_back(0);
      }
      else {
        auto &forward_dict = forward_chk[query_size];
        int result = forwardSearch(forward_dict, query, 0);

        answer.push_back(result);
      }
    }
    else { // backward search
      if(backward_chk.find(query_size) == backward_chk.end()) {
        answer.push_back(0);
      }
      else {
        auto &backward_dict = backward_chk[query_size];
        int result = backwardSearch(backward_dict, query, (int)query.size() - 1);

        answer.push_back(result);
      }
    }
  }

  return answer;
}
```

이 문제를 풀고 나니 시간이 20분 정도 남았던 것으로 기억해요. 그래서 나머지 문제는 풀지 못했네요.

## 오프라인 코딩 테스트

떨어질 줄 알았던 온라인 코딩 테스트를 통과했어요. 그래서 오프라인 코딩 테스트를 준비하게 되었어요.

![온라인 코딩 테스트 안내 메일](/assets/images/2019-10-16-2020-kakao-blind-recruitment/Screen-Shot-2019-10-15-at-9.25.46-PM.png)

오프라인 테스트는 온라인 테스트와 다르게 흔히 보던 PS 문제가 아니었어요.

일반적인 PS 문제는

- STDIN 을 통해 데이터를 입력받고 STDOUT 으로 결과를 출력
- 특정 function을 작성하여 function args 로 데이터를 받고 결과를 return

이런 방식이지요. 거의 모든 PS 채점 사이트는 위 두가지 방식 중 하나(혹은 둘 다)를 사용하고 있어요.

오프라인 테스트는 HTTP 요청/응답 을 통해 데이터를 주고받아요. 문제를 푸는 데 필요한 데이터는 HTTP 응답을 통해 받을 수 있으며 이를 이용해 얻은 결과를 HTTP 요청을 통해 보내는 방식이에요.

그리고 중요한 점으로 이 방식에서는 데이터가 한번에 전부 주어지지 않아요. 매 요청마다 전체 데이터의 일부를 조금씩 받는 방식이에요. (물론 문제에 따라 차이는 있어요). 그리고 매번 주어지는 데이터는 매번 얻은 결과에 따라서 다르게 주어질 수도 있어요.

뭔 개소리냐고 생각하시는 분들이 많을 거에요. 저가 봐도 이 글만 보면 뭔 말인지 글쎄요.

이해를 돕기 위해 이전에 나왔던 문제 링크를 아래에 적어놨어요.  
(글 작성 시점에서 이번 오프라인 테스트의 해설이 올라오지 않아서 작년 문제 링크를 올려놨어요)

- [2019 카카오 블라인드 공채 2차 오프라인 코딩 테스트 문제 해설](https://tech.kakao.com/2018/10/23/kakao-blind-recruitment-round-2/)
- [관련 GitHub Repository](https://github.com/kakao-recruit/2019-blind-2nd-elevator)

![오프라인 코딩 테스트 좌석 명찰](/assets/images/2019-10-16-2020-kakao-blind-recruitment/IMG_0223-1.jpeg)

오프라인 테스트는 카카오 판교오피스 인근에 있는 거대한 강당에서 진행되었어요.  
(나중에 들은 이야기인데 여기 말고 몇 개 더 다른 장소에서도 테스트 진행이 있었던 것 같아요)

이날 순서로는

- Orientation
- 서술형 문답
  - 평가에 반영되지는 않으며 지원자의 관심도 조사를 위한 것이라고 했어요.
- 객관식/단답형 테스트 (20분)
  - Computer Science 기본 전공 지식을 묻는 문제들로 구성되어 있어요.
  - 기본적인 자료구조/알고리즘, 운영체제, DB 등 전공 관련 내용이 나왔던 것으로 기억해요. 학교 수업을 잘 들었다면 별다른 고민 없이 쉽게 풀 수 있어요.
- 오프라인 코딩 테스트 (4시간 30분)
  - 이게 사실상 메인이죠.
  - 이번에는 2문제가 나왔어요.
  - 데이터 크기가 작은 첫 번째 문제와 데이터 크기가 큰 두 번째 문제로 구성되어 있어요.
  - 두 문제는 데이터 크기 등 세부 사항에서 차이가 있으나 기본적으로 같은 문제였어요.

초반에 삽질을 많이 해서 1시간 30분정도를 그냥 잡아먹었어요. 다행히도 삽질을 일찍 끝내고 1번 문제를 풀었어요.

문제는 2번이었어요. 1번 문제를 풀 때 작성한 코드는 2번에서도 수정 없이 작동하도록 했지만 작성한 코드의 시간복잡도가 좋지 않았고 주어지는 데이터의 크기가 생각보다 커서 실행된 코드가 언제 끝날지를 모르겠는거에요. 그래서 일단은 실행을 해둔 채로 두고 코드 최적화를 시도했어요.

다행히도 테스트 종료 10분 전에 2번째 문제의 해결에 성공했어요. (한 15분동안 내내 돌아갔던거 같아요)

이날 작성했던 코드는 문제 해설이 올라오는 대로 추가할 예정이에요.

### 1번 문제 결과

```json
{
  "score": {
    "accepted_recommend": 2000,
    "day": 47,
    "expected_recommend_rate": 48.88999,
    "finished_at": "2019-09-21T07:32:12.406+00:00",
    "finished_user_count": 100,
    "original_token": "8dc5c7c27ca85028dc42a206b0d944ca",
    "problem": 1,
    "rejected_recommend": 2027,
    "score": 19.062,
    "status": "finish",
    "token": "f388ad3b-cf4d-42d8-bf90-d4e759147131",
    "total_recommend": 4027,
    "username": "8dc5c7"
  }
}
```

### 2번 문제 결과

```json
{
  "score": {
    "accepted_recommend": 98120,
    "day": 52,
    "expected_recommend_rate": 56.31548,
    "finished_at": "2019-09-21T08:51:26.968+00:00",
    "finished_user_count": 4906,
    "original_token": "8dc5c7c27ca85028dc42a206b0d944ca",
    "problem": 2,
    "rejected_recommend": 75874,
    "score": 52.29812,
    "status": "finish",
    "token": "2b0b399b-a773-450d-b3ba-43e65a939c7b",
    "total_recommend": 173994,
    "username": "8dc5c7"
  }
}
```

다만 푸는 데 시간이 오래 걸렸던 관계로 과연 오프라인 테스트 통과가 가능할지는 의문이었죠.

## 1차 면접

오프라인 테스트도 어떻게 결국 통과를 했어요.

이후로는 이제 지원한 계열사에 따라 면접 과정이 완전히 달라져요. (저는 카카오에 지원을 했어요)

![1차 면접 안내 메일 1](/assets/images/2019-10-16-2020-kakao-blind-recruitment/Screen-Shot-2019-10-16-at-11.38.09-AM.png)

처음 지원할 때는 이름과 이메일, 연락처만 적었기 때문에 추가적인 정보를 요구해요. 들어가게 되면 개발 경험 등을 추가적으로 적게 되어요.

블라인드 채용이다 보니 학력을 적는 칸은 존재하지 않았어요. 다만 졸업 예정 시기를 적는 칸은 있었어요.

![1차 면접 안내 메일 2](/assets/images/2019-10-16-2020-kakao-blind-recruitment/Screen-Shot-2019-10-16-at-11.42.19-AM.png)

<!-- **"토론형 인터뷰"** 가 뭔지 참 궁금했어요. (왜냐면 면접 경험이 별로 없었기 때문이죠)

당일날 일정은 다음과 같이 진행되었어요.

1. Orientation
2. 면접 문제를 받고 30분동안 준비
3. 면접관 2명, 지원자 1명 비율로 면접 진행
4. 인성검사 (심리테스트 같이 객관식 형식이며 1차 면접 결과에 영향을 주지 않는다고 했어요.)
5. (Optional) 오피스 투어 -->

<!-- 면접 문제 내용은 대략 아래와 같았어요.

> 회사에서 어떤 서비스 A와 B를 개발해야 한다고 합니다.
> 여건상 둘 다 선택할 수는 없으며 한 가지만 선택할 수 있다고 합니다.
> 면접관은 회사에서 결정권자의 역할을 하며 지원자는 결정권자를 설득하는 역할을 합니다.
> 현재 회사의 상태(어떤 기술에서 강점인지, 최근 시장 점유율 변화 등)와 경쟁사의 상태 등이 자료로 주어집니다. (물론 전부 가상의 설정입니다)
> 이 자료들을 기반으로 지원자는 둘 중 하나를 선택한 후 선택에 대한 이유를 함께 설명하여 설득을 진행해야 합니다.

보자마자 뇌정지가 왔어요. 이런 쪽으로는 경험은 커녕 접해본 적도 없었거든요.
30분동안 생각을 하고 면접 들어가야 하는데 음... 뭐라고 표현해야 할 지 모르겠어요.

당연히 면접 진행하면서 "아 망했다" 생각만 계속 들었어요. (그리고 결국 망했어요)

1차 면접에서는 따로 전공지식이나 기술에 대한 질문은 없었어요. -->

## 결과

혹시? 했지만 역시 떨어졌어요.

![망했어요](/assets/images/2019-10-16-2020-kakao-blind-recruitment/Screen-Shot-2019-10-16-at-12.03.38-PM.png)

그래도 좋은 경험이 되었어요. 앞으로 이런 유형의 면접에 대해서도 준비를 좀 해야 할 것이고 코딩 테스트도 문제를 더 잘 풀 수 있도록 연습을 해야 하겠지요.

개인적으로 오프라인 코딩 테스트 방식이 많이 흥미로웠어요. 이런 방식의 대회가 있으면 재미있을 것 같아요.

긴 글 읽어주셔서 감사합니다.
