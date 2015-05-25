#### Tabs are used for indentation (not spaces)

    (tab)printf("hello world"); // correct
         printf("hello world"); // incorrect

Currently we use tabs instead of spaces. However we are considering
using 2 spaces instead of tabs in the future, since tab sizing can be
changed (people often change their tab size to 2), which results in
inconsistent tab sizes between different editors.

#### Newline/LF (\\n) used for line endings

Always use \*nix line feed (\\n) for line endings. Never use Mac CR
(\\r) or Windows CR/LF (\\r\\n). Most of the time, git will handle this
automagically.

#### Always end files in a new line

As a courtesy to command line users (using cat/type/more/less commands),
files should always end in a new line so that the command prompt looks
tidy.

#### Member names are tab indented to column 25

    class CMyClass {
    public:
        CFoobar             m_foobar;
        CFoobar             foobar();
        CReallyReallyLongClassName
                            m_memberOnNextLine;
    };

#### Arguments are tab indented twice, ctor init list only once

    CMyClass::CMyClass(
            CFoobar* foobar,
            CMoreStuff* moreStuff) :
        m_foobar(foobar)
    {
        // ...
    }

#### Class, struct, and enum names are `PascalCase` and prefixed

    class CMyClass { }; // class prefix is C
    struct CMyStruct { }; // struct prefix is C
    enum EMyEnum { }; // enum prefix is E

#### Member, and enum constant names are lower `camelCase`

    class CMyClass {
    public:
        void                helloWorld();
    };

#### Member variables (public and private) begin with `m_`

    class CMyClass {
    public:
        CMyClass*           m_myClass1;
    private:
        CMyClass*           m_myClass2;
    };

#### Enum constants are prefixed with `k`

    enum EMyEnum {
        kValue1,
        kValue2
    };

#### Static variables are prefixed with `s_`

    static CMyClass*       s_myClass;

#### Comments and debug messages need not be grammatically correct

    // i'm using bad grammar. but i like to use full stops
    DEBUG((CLOG_INFO "hello world"));

#### Multiline comments use single-line commenting (`//`)

    // for long block comments, instead of using the slash asterisk
    // comments, we use the two-slash comments

#### Function return types must go on the line above the function name

    // correct
    int
    CMyClass::helloWorld()
    {
    }

    // incorrect
    int CMyClass::helloWorld()
    {
    }

#### For functions, curly braces start on the next line after the function name

    // correct
    int
    CMyClass::helloWorld()
    {
        // ...
    }

    // incorrect
    int
    CMyClass::helloWorld() {
        // ...
    }

#### For conditional statements, curly braces start on the same line

    // correct
    if (a == b) {
        // ...
    }

    // correct
    for (int i = 0; i < a; i++) {
        // ...
    }

    // incorrect
    if (a == b)
    {
        // ...
    }

    // incorrect
    for (int i = 0; i < a; i++)
    {
        // ...
    }

#### A space is used between operators and operands

    // correct
    a = b;
    if (c == d) {
    }

    // incorrect
    a=b;
    if (c==d) {
    }

#### No space is inserted after or before conditions within parenthesis

    // correct
    if (a == b) {
        // ...
    }

    // incorrect
    if ( a == b ) {
        // ...
    }

#### The void keyword is not used for functions that do not take parameters

    // correct
    void helloWorld();

    // incorrect
    void helloWorld(void);

#### The const keyword is used regularly

    class CMyClass {
    public:
        const char*         helloWorld();
    };

    CMyClass::CMyClass(const char* helloWorld)
    {
    }

#### Spaces must exist between conditional statement parenthesis and the keyword

    // correct
    if (a == b) {
    }

    // incorrect
    if(a == b) {
    }

#### No spaces between type and reference and pointer operator

    // correct
    char* helloWorld;

    // incorrect
    char *helloWorld;

#### Pre-processor commands within \#if are indented

    // correct
    #if HELLO_WORLD
    #    include "HellWorld.h"
    #endif

    // incorrect
    #if HELLO_WORLD
    #include "HellWorld.h"
    #endif

#### Pointers and references do not have prefixes

    // correct
    CMyClass* myClass1 = new CMyClass();
    CMyClass& myClass2 = *myClass1;

    // incorrect
    CMyClass* p_myClass1 = new CMyClass();
    CMyClass& p_myClass2 = *p_myClass1;

    // incorrect
    CMyClass* pMyClass1 = new CMyClass();
    CMyClass& pMyClass2 = *pMyClass1;

#### The left comparator operand is a variable

    // correct
    if (a == "hello world") {
    }

    // incorrect
    if ("hello world" == a) {
    }

#### The else statement always goes on a new line

    // correct
    if (a == b) {
      // ...
    }
    else {
      // ...
    }

    // incorrect
    if (a == b) {
      // ...
    } else {
      // ...
    }

#### Function arguments indent once when wrapped

    void
    foobar() {
      functionWithManyArgs(
        arg1,
        arg2,
        arg3);
    }

#### Case switch lines up with switch

    switch (type) {
    case kFoobar:
      break;
    }

#### Curly brace for case on same line

    case kFoobar: {
      // ...
      break;
    }

#### Includes must use the full relative path

    // correct
    #include "arch/win32/ArchInternetWindows.h"

    // incorrect
    #include "ArchInternetWindows.h"

#### Includes are ordered: pair, local, system

    #include "arch/win32/ArchInternetWindows.h" // matching source/header pair

    #include "arch/win32/XArchWindows.h" // arch/win32 could depend on arch
    #include "arch/Arch.h" // arch could depend on base
    #include "base/Log.h" // base could depend on common
    #include "common/Version.h" // probably has the least dependencies

    #include <sstream> // system (after divide)
    #include <Wininet.h> // system

**Rationale:** This ordering is to reduce accidental dependency solving,
so that headers are as independent as possible. For example, including a
system header (e.g. `sstream`) before a local include (e.g.
`ArchInternetWindows.h`) may accidentally solve a dependency (headers
should include their own dependencies). There should be a clear divide
between each group (pair, local and system) so that its easy to see the
pair header is first, and it also reduces the possibility of local
headers being “tagged on” to the end of the include list, which could
introduce accidental dependencies; someone who hasn't read this guide is
more likely to ask, “Why is there a divide here?”

#### Use \#pragma once header guard

    // correct
    #pragma once

    // incorrect
    #ifndef OLD_STYLE_GUARD_H
    #define OLD_STYLE_GUARD_H
    // ...
    #endif // OLD_STYLE_GUARD_H

**Rationale:** `#pragma once` is very well supported across compilers
but not actually part of the standard. The preprocessor may be a little
faster with it as it is more simple to understand your exact intent.
`#pragma once` is less prone to making mistakes and it is less code to
type.

#### Put system header macros after local includes

    //
    // correct
    //
    #include "foo/Bar.h"

    #define WIN32_LEAN_AND_MEAN
    #include <Windows.h>

    //
    // incorrect
    //
    #define WIN32_LEAN_AND_MEAN

    #include "foo/Bar.h"

    #include <Windows.h>

**Rationale:** Though unlikely, local header (e.g. `Bar.h`) may also
include `Windows.h` but needs the features that the
`WIN32_LEAN_AND_MEAN` macro removes. This is probably the worst example,
as you almost always want to use `#define WIN32_LEAN_AND_MEAN` (except
for when using `IUnknown`, for example).

#### Non-class files are lower case with underscore

    // correct
    #include "example/foo_bar.h" // no classes here!

    # incorrect
    #include "example/FooBar.h" // no classes here!

**Rationale:** Upper camel case naming for non-class names is
potentially misleading, as it suggests that the file contains a class.

#### Treat acronyms as words

    // correct
    TcpFoobar m_tcpFoobar;
    FoobarTcp m_foobarTcp;

    # incorrect
    TCPFoobar m_TCPFoobar;
    FoobarTCP m_FoobarTCP;

**Rationale:** TCPFoobar could be misread as TCPF-oobar, and members are
always lowerCamel.

#### Return from expression instead of returning true or false

    // correct
    return a == b;

    // incorrect
    if (a) {
        return true;
    }
    else {
        return false;
    }

#### Prefer throwing rather than returning error codes

    // correct
    throw GreatDisturbanceInForce(
        "millions of voices suddenly cried out in terror, "
        "and were suddenly silenced");
    throw MonkiesHaveTakenOverThePlanet(monkeyCount);

    // incorrect
    return -1; // something bad happened
    return 42; // the wrong question was asked

#### If error codes must be used, use constants instead of magic numbers

    // correct
    enum EFoobarErrors {
        kInvalidCapitalOfCountry,
        kInvalidFavoriteColor
    }
    if (favoriteColor == kColorBlue) {
        return kInvalidFavoriteColor;
    }

    // incorrect
    if (capitalOfAssyria == NULL) {
        return -1; // invalid capital of country
    }