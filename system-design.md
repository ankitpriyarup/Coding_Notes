# System Design

## Design Patterns

### Singleton Class

```cpp
class Restaurant
{
private:
    static Restaurant _instance = NULL;
public:
    static Restaurant getInstance()
    {
        if (!_instance) _instance = Restaurant();
        return _instance;
    }
};
```

> Can interfere unit testing - provide a well-known point of access to some service in your application so that you don’t have to pass around a reference to that service. How is that different from a global variable? \(remember, globals are bad, right???\) What ends up happening is that the dependencies in your design are hidden inside the code, and not visible by examining the interfaces of your classes and methods. You have to inspect the code to understand exactly what other objects your class uses.

### Factory Method

```cpp
class CardGame
{
public:
    static CardGame createCardGame(GameType type)
    {
        if (type == GameType::POKER) return new PokerGame();
        else if (type == GameType::BLACKJACK) return new Blackjack();
        return NULL;
    }
};
```

## Common Questions

### Deck of Cards

Design the data structure for a generic deck of cards. Explain how would you subclass the data structures to implement blackjack.

```cpp

```

## Grokking The System Design Interviews

Part 1: [https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-1/579](https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-1/579)  
Part 2: [https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-2/580](https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-2/580)  
Part 3: [https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-3/581](https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-3/581)  
Part 4: [https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-4/583](https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-4/583)  
Part 5: [https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-5/584](https://coursehunters.online/t/educative-io-design-gurus-grokking-the-system-design-interview-part-5/584)

