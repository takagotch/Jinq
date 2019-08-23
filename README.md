### jinq
---
https://github.com/my2iu/Jinq

```java
// api/test/org/jinq/test/query/NonQueryJinqStreamTest.java

public class NonQueryJinqStreamTest
{
  @Before
  public void setUp() throws Exception
  {
  }
  
  @After
  public void tearDown()
  {
  }
  
  @Test
  public void testGroup()
  {
    JinqStream<Pair<integer, Integer>> stream =
      new NonQueryJinqStream<>(Stream.of(
        new Pair<>(1,2), new Pair<>(1, 3), new Pair<>(2, 5)));
    List<Pair<Integer, Integer>> results = stream
      .group(pair -> pair.getOne(), (key, pairs) -> pairs.<Integer>max(x -> x.getTwo()))
      .toList();
    assertEquals(2, results.size());
    assertTrue(results.contains(new Pair<>(1, 3)));
    assertTrue(results.contains(new Pair<>(2, 5)));
  }
  
  @Test
  public void testAggregate()
  {
    JinqStream<Integer> stream =
      new NonQueryJinqStream<>(Stream.of(1, 2, 3, 4, 5));
    Tuple3<Long, Integer, Long> result =
      stream.aggregate((vals) -> vals.sumInteger(x -> x),
        (vals) -> vals.max(x -> x),
        (vals) -> vals.sumInteger(x -> x + 1));
    assertEquals(15, result.getOne().intValue());
    assertEquals(5, result.getTwo().intValue());
    assertEquals(20, result.getThree().intValue());
  }
  
  @Test
  public void testSum()
  {
    assertEquals(15, (long)new NonQueryJinqStream<>( Stream.of(1, 2, 3, 4, 5)).sumInteger(n -> n));
    assertEquals(Math.abs(20.0 - new NonQueryJinqStream<>( Stream.of(1, 2, 3, 4, 5)).sumDouble(n -> n + 1.0)) < 0.01);
  }
  
  @Test
  public void testMax()
  {}
  
  @Test
  public void testLeftOuterJoinOn()
  {
    Pair<Integer, Integer>[] vals =
      new NonQueryJinqStream<>( Stream.of(1, 2) )
        .leftOuterJoin(
          (val, source) -> new NonQueryJinqStream<>( Stream.of(1, 3) ),
          (a, b) -> a == b
        )
        .toList()
        .toArray(new Pair[0]);
    assertArrayEuals(
      new Pair[] {
        new Pair<>(1, 1),
        new Pair<>(2, null)
      }, vals);
  }
}

```

```
```

```
```


