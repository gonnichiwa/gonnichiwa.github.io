---
title: java의 delegating(위임) 예시
date: 2024-05-31 12:35:32 +0900
categories: [JAVA]
tags: [java, designpattern]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 예제

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

// client
public class DelegatingTest {

    /* delegate : 클라이언트는 delegate만 호출할 뿐, 직접 */
    @Test
    public void catdogsound(){
        AnimalDelegator ad = new AnimalDelegator(); // 기능 수행의 위임자를 불러옴.
        assertThat(ad.makeSound(AnimalKind.CAT), is("cat sound")); // client입장에서 필요한 기능
        assertThat(ad.makeSound(AnimalKind.DOG), is("dog sound"));
    }
}
// delegator : client 에게 concreate(Dog, Cat) 의 기능을 공급.
class AnimalDelegator {
    List<Animal> animals;

    public AnimalDelegator() {
        animals = new ArrayList<>();
        animals.add(new Cat());
        animals.add(new Dog());
    }

    public String makeSound(AnimalKind kind) { // client입장에서 필요한 기능
        return animals.stream()
                .filter(a -> a.isSameKind(kind))
                .findFirst().get().makeSound();
    }
}


interface Animal {
    String makeSound();
    AnimalKind getKind();
    boolean isSameKind(AnimalKind animalKind);
}

enum AnimalKind {
    CAT, DOG
}
// concreate
class Cat implements Animal {

    @Override
    public String makeSound() {
        return "cat sound";
    }

    @Override
    public AnimalKind getKind() {
        return AnimalKind.CAT;
    }

    @Override
    public boolean isSameKind(AnimalKind animalKind) {
        return AnimalKind.CAT.equals(animalKind);
    }
}
// concreate
class Dog implements Animal {
    @Override
    public String makeSound() {
        return "dog sound";
    }

    @Override
    public AnimalKind getKind() {
        return AnimalKind.DOG;
    }

    @Override
    public boolean isSameKind(AnimalKind animalKind) {
        return AnimalKind.DOG.equals(animalKind);
    }
}
```

### 참고

https://ssdragon.tistory.com/139