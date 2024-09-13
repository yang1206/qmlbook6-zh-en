# The QObject

As described in the introduction, the `QObject` is what enables many of Qt's core functions such as signals and slots. This is implemented through introspection, which is what `QObject` provides. `QObject` is the base class of almost all classes in Qt. Exceptions are value types such as `QColor`, `QString` and `QList`.

正如简介中所述，`QObject` 实现了 Qt 核心功能，如信号和槽。这是通过内省实现的，这是 `QObject` 提供的。`QObject` 几乎是 Qt 中所有类的基类。例外是值类型，如 `QColor`、`QString` 和 `QList`。

A Qt object is a standard C++ object, but with more abilities. These can be divided into two groups: introspection and memory management. The first means that a Qt object is aware of its class name, its relationship to other classes, as well as its methods and properties. The memory management concept means that each Qt object can be the parent of child objects. The parent *owns* the children, and when the parent is destroyed, it is responsible for destroying its children.

Qt 对象是一个标准的 C++ 对象，但具有更多功能。这些可以分为两组：内省和内存管理。第一个意味着 Qt 对象知道其类名、与其他类的关联以及其方法和属性。内存管理概念意味着每个 Qt 对象可以是子对象的父对象。父对象 *拥有* 子对象，当父对象被销毁时，它负责销毁其子对象。


The best way of understanding how the `QObject` abilities affect a class is to take a standard C++ class and Qt enables it. The class shown below represents an ordinary such class.

理解 `QObject` 能力如何影响类最好的方式是取一个标准的 C++ 类并启用它。下面显示的类就是一个这样的普通类。


The person class is a data class with a name and gender properties. The person class uses Qt’s object system to add meta information to the c++ class. It allows users of a person object to connect to the slots and get notified when the properties get changed.

`Person` 类是一个具有名称和性别属性的数据类。`Person` 类使用 Qt 的对象系统向 c++ 类添加元信息。它允许用户连接到槽并在属性更改时得到通知。

```cpp
class Person : public QObject
{
    Q_OBJECT // enabled meta object abilities

    // property declarations required for QML
    Q_PROPERTY(QString name READ name WRITE setName NOTIFY nameChanged)
    Q_PROPERTY(Gender gender READ gender WRITE setGender NOTIFY genderChanged)

    // enables enum introspections
    Q_ENUM(Gender)
    
    // makes the type creatable in QML
    QML_ELEMENT

public:
    // standard Qt constructor with parent for memory management
    Person(QObject *parent = 0);

    enum Gender { Unknown, Male, Female, Other };

    QString name() const;
    Gender gender() const;

public slots: // slots can be connected to signals, or called
    void setName(const QString &);
    void setGender(Gender);

signals: // signals can be emitted
    void nameChanged(const QString &name);
    void genderChanged(Gender gender);

private:
    // data members
    QString m_name;
    Gender m_gender;
};
```

The constructor passes the parent to the superclass and initializes the members. Qt’s value classes are automatically initialized. In this case `QString` will initialize to a null string (`QString::isNull()`) and the gender member will explicitly initialize to the unknown gender.

构造函数将父对象传递给超类并初始化成员。Qt 的值类会自动初始化。在这种情况下，`QString` 将初始化为空字符串（`QString::isNull()`），性别成员将显式初始化为未知性别。

```cpp
Person::Person(QObject *parent)
    : QObject(parent)
    , m_gender(Person::Unknown)
{
}
```

The getter function is named after the property and is normally a basic `const` function. The setter emits the changed signal when the property has changed. To ensure that the value actually has changed, we insert a guard to compare the current value with the new value. Only when the value differs we assign it to the member variable and emit the changed signal.

获取函数以属性命名，通常是一个基本的 `const` 函数。当属性发生变化时，设置器会发出更改信号。为了确保值实际上已经改变，我们插入一个防护程序来比较当前值和新的值。只有当值不同时，我们才将其分配给成员变量并发出更改信号。

```cpp
QString Person::name() const
{
    return m_name;
}

void Person::setName(const QString &name)
{
    if (m_name != name) // guard
    {
        m_name = name;
        emit nameChanged(m_name);
    }
}
```

Having a class derived from `QObject`, we have gained more meta object abilities we can explore using the `metaObject()` method. For example, retrieving the class name from the object.

从 `QObject` 派生的类，我们获得了更多的元对象能力，我们可以使用 `metaObject()` 方法来探索。例如，从对象中检索类名。



```cpp
Person* person = new Person();
person->metaObject()->className(); // "Person"
Person::staticMetaObject.className(); // "Person"
```

There are many more features which can be accessed by the `QObject` base class and the meta object. Please check out the `QMetaObject` documentation.

`QObject` 基类和元对象可以访问许多其他功能。请查看 `QMetaObject` 文档。

::: tip
`QObject`, and the `Q_OBJECT` macro has a lightweight sibling: `Q_GADGET`. The `Q_GADGET` macro can be inserted in the private section of non-`QObject`-derived classes to expose properties and invokable methods. Beware that a `Q_GADGET` object cannot have signals, so the properties cannot provide a change notification signal. Still, this can be useful to provide a QML-like interface to data structures exposed from C++ to QML without invoking the cost of a fully fledged `QObject`.

`QObject` 和 `Q_OBJECT` 宏有一个轻量级的兄弟：`Q_GADGET`。`Q_GADGET` 宏可以插入非 `QObject` 派生类的私有部分，以公开属性和可调用方法。请注意，`Q_GADGET` 对象不能有信号，因此属性不能提供更改通知信号。尽管如此，这仍然可以用来提供类似于 QML 的接口，将 C++ 暴露给 QML 的数据结构，而不需要完全成熟的 `QObject` 的成本。
:::
:::

