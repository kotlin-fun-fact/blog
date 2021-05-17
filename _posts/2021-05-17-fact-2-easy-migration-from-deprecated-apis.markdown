---
layout: post
title:  "Fact #2: Easy migration from deprecated APIs"
date:   2021-05-17 09:18:12 +0200
categories: jekyll update
---
**TL;DR**: Alt+Enter is your friend when moving away from deprecated APIs in Kotlin.

Deprecations happen when library authors come up with a better way to handle a specific use case. It often saves library
maintainers' time because of no further need to support the old API, but creates an extra overhead for library
consumers because they need to migrate their code.

Kotlin has a nice little feature that facilitates this process. For example, the recent deprecations around Time API
resulted in such warning when switching to Kotlin 1.5.0:

![Deprecated Time API]({{site.baseurl}}/assets/deprecation-use-site.png)

The clue is that together with the deprecation notice, **IntelliJ offers an action to replace with the new recommended
API**. This is possible thanks to the extra piece of info in the `@Deprecated` annotation:

{% highlight kotlin %}
@Deprecated(
    "Use lowercase() instead.",
    ReplaceWith("lowercase(Locale.getDefault())", "java.util.Locale"))
{% endhighlight %}

where `ReplaceWith` is defined like this:

{% highlight kotlin %}
/**
* Specifies a code fragment that can be used to replace a deprecated function, property or class. Tools such
* as IDEs can automatically apply the replacements specified through this annotation.
*
* @property expression the replacement expression. The replacement expression is interpreted in the context
*     of the symbol being used, and can reference members of enclosing classes etc.
*     For function calls, the replacement expression may contain argument names of the deprecated function,
*     which will be substituted with actual parameters used in the call being updated. The imports used in the file
*     containing the deprecated function or property are NOT accessible; if the replacement expression refers
*     on any of those imports, they need to be specified explicitly in the [imports] parameter.
* @property imports the qualified names that need to be imported in order for the references in the
*     replacement expression to be resolved correctly.
*/
@Target()
@Retention(BINARY)
@MustBeDocumented
public annotation class ReplaceWith(val expression: String, vararg val imports: String)
{% endhighlight %}

It's helpful for both sides of the library API: author and consumer. IntelliJ doesn't have monopoly to suggest such
replacements - it can ba used by any tool, maybe [ktlint](https://github.com/pinterest/ktlint) or
[detekt](https://github.com/detekt/detekt) in the future.
