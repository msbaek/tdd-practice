# Move#getAverage

[Test Driven Development: By Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)의 예제

사용자들은 영화에 평점을 부여할 수 있고(rate), 평점의 평균(getAverage)을 얻을 수 있다.

## source directory 생성

```language
mkdir -p src/main/java
mkdir -p src/test/java
```

## Movie Class 생성

바로 src/main/java 디렉토리에 Movie.java를 만들고 싶지만...
그럴 필요가 있게 하는 것이 먼저다.


```java
publc class MovieTest {
    @Test
    void foo() {
    }
}
```

compile, run 되는지 확인

```java
    @Test
    void canCreateMovie() {
        new Movie();
    }
```

F2, Show Intention Actions(opt+enter)로 quick fix

split vertically

![Alt text](https://monosnap.com/image/qiOJm8QDAyUoX76iMbslTbQY66l5BJ.png)

## average for 0 rating

제일 단순하지만 흥미있는 테스트는 한번도 rate되지 않은 경우

```java
    @Test
    @DisplayName("한번도 rate되지 않으면 average는 0")
    void should_return_0_when_no_ratings() {
        Movie movie = new Movie();
        assertThat(movie.getAverage()).isEqualTo(0);
    }
```

return 0 ok
return -1 no ok
  
우리 테스트는 성공하는 경우와 실패하는 경우 모두 동작
  
## average for 1 rating

그 다음으로 단순하지만 흥미로운 테스트는 한번만 rate하는 경우

복붙으로 시작

```java
    @Test
    @DisplayName("한번만 rate되면 average는 rate된 값")
    void should_return_1_when_1_was_rated() {
        Movie movie = new Movie();
        movie.rate(1);
        assertThat(movie.getAverage()).isEqualTo(1);
    }
```

test 이름이 composed method pattern. step down rule

return 1 ???

rate 필드 변수에 할당 & 반환

## average for 2 ratings

그 다음으로 단순하지만 흥미로운 테스트는 두번 rate하는 경우

이게 평점을 계산하는 알고리즘이다

## result

```java
public class MovieTest {
    private Movie movie;

    @BeforeEach
    void setUp() {
        movie = new Movie();
    }

    @Test
    @DisplayName("한번도 rate되지 않으면 average는 0")
    void should_return_0_when_no_ratings() {
        assertThat(movie.getAverage()).isEqualTo(0);
    }

    @Test
    @DisplayName("한번만 rate되면 average는 rate된 값")
    void should_return_1_when_1_was_rated() {
        movie.rate(1);
        assertThat(movie.getAverage()).isEqualTo(1);
    }

    @Test
    @DisplayName("두번만 rate되면 average는 rate된 값의 합의 평균값")
    void should_return_2_when_1_and_3_were_rated() {
        movie.rate(1);
        movie.rate(3);
        assertThat(movie.getAverage()).isEqualTo(2);
    }
}
```