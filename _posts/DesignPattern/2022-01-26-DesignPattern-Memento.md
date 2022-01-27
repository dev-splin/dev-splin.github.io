---
title: "Design Pattern : 메멘토(Memento) 패턴"
excerpt_separator: <!--more-->
categories:
  - Design Pattern
tags:
  - Design Pattern
  - "Design Pattern : Memento"
toc: true
toc_sticky: true
toc_label: 목차 
---

# 메멘토(Memento) 패턴

메멘토 패턴은 **캡슐화를 유지하면서 객체 내부 상태를 외부에 저장하는 패턴**입니다.



## 다이어그램

![memento](https://user-images.githubusercontent.com/79291114/151184142-6a60b443-3ba4-4eea-855a-981aaa82d9fd.PNG)

- `Originator` : 객체의 정보를 가지고 있는 오리지널 객체
  - 제 3자가 객체 내부 정보를 알지 못하게 하기 위하여 **Memento를 만들 수 있는 메서드가 필요**함
- `Memento` : 오리지널 객체의 정보를 담는 불변 객체
- `CareTaker` : 오리지널이 반환한 **Memento를 가지고 있다가 필요할 때 Memento의 값을 Originator에게 전달**함





## 예시 코드

너무 간단한 패턴이라 코드만 봐도 쉽게 이해할 수 있을 거라 생각합니다.



### 메멘토 패턴 적용 전

Game 객체의 정보를 복원하기 위하여 저장을 해야할 때 **외부에 객체의 정보를 노출함으로써 캡슐화를 지킬 수 없게 됩니다.**

```java
// 게임 객체
public class Game implements Serializable {

    private int redTeamScore;

    private int blueTeamScore;

    public int getRedTeamScore() {
        return redTeamScore;
    }

    public void setRedTeamScore(int redTeamScore) {
        this.redTeamScore = redTeamScore;
    }

    public int getBlueTeamScore() {
        return blueTeamScore;
    }

    public void setBlueTeamScore(int blueTeamScore) {
        this.blueTeamScore = blueTeamScore;
    }
}

// 클라이언트
public class Client {

    public static void main(String[] args) {
        Game game = new Game();
        game.setRedTeamScore(10);
        game.setBlueTeamScore(20);
        
        // 게임을 복원하기 위해서 게임의 정보를 노출
        int blueTeamScore = game.getBlueTeamScore();
        int redTeamScore = game.getRedTeamScore();

        // 외부에 저장한 게임의 정보를 그대로 가져와서 새로운 게임을 만듬 -> 캡슐화 깨짐
        Game restoredGame = new Game();
        restoredGame.setBlueTeamScore(blueTeamScore);
        restoredGame.setRedTeamScore(redTeamScore);
    }
}
```



---

참고 : [코딩으로 학습하는 GoF의 디자인 패턴](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4/dashboard)
