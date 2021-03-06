Play module intends to speed up development of large projects with Play 1 framework.

This is a work in progress.

The aim is to:
* disable enhancers
* disable Play's own compilation of Java code
* simplify classloading

## How to migrate from "classic" Play1 application to play-rebel

### Find all "classic" redirects

1. Add this line to your `play.plugins`:

```
0:play.rebel.PlayRebelAntiEnhancerPlugin
2000:play.rebel.migration.LegacyRedirectsDetector
```

2. Start your Play application
3. Find phrase `legacy redirects` in logs
4. Add this line to every controller:

```java
  protected static Redirector redirect = new Redirector();
```

4. Replace every call to "action" method, like `SomeController.someAction()` by 

```java
  redirect.to("SomeController.someAction");
```

5. Replace every call to "action" method with parameters, like `SomeController.someAction(42, "yip")` by 

```java
  redirect.with("userId", 42).with("password", "yip").to("SomeController.someAction");
```

6. Replace every call to `Validation.valid(Object)` with a call to `Validation.valid(String, Object)`
7. Don't use methods:
 * `play.db.jpa.GenericModel.validateAndSave()`
 * `play.db.jpa.GenericModel.validateAndCreate()`
