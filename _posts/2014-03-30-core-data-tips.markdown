---
layout: post
title:  "Core Data tip"
date:   2014-03-30 19:41:20
---

As a first post, just a quick Core Data tip to get started.

While working with the managed objects, one quickly ends up using a bunch of hardcoded strings for property and class names. Typical examples include

{% highlight objective-c %}
[NSEntityDescription insertNewObjectForEntityForName:@"EntityName" 
						      inManagedObjectContext:context];
{% endhighlight %}
or 
{% highlight objective-c %}
[NSPredicate predicateWithFormat:@"propertyName = %@", value];
{% endhighlight %}

The former can be mitigated e.g. by using a common superclass for all the entity with a method like 
{% highlight objective-c %}
+ (NSString *)entityName {
	return NSStringFromClass(self);
}
{% endhighlight %}
as long as the name of the entity always corresponds to the name of the backing class (which is a reasonable default anyway). Even if some class is called differently, it can choose to override this method and still avoid hardcoding the string everywhere it is used.

The latter can be dealt with elegantly by using a feature of the `NSPredicate` format:
{% highlight objective-c %}
[NSPredicate predicateWithFormat:@"%K = %@", PropertyName, value];
{% endhighlight %}
where `PropertyName` could be a constant defined in the class, making it somewhat easier to spot refactoring mistakes. One way to quickly define the necessary constants is to use the [Accessorizer](http://www.kevincallahan.org/software/accessorizer.html).

