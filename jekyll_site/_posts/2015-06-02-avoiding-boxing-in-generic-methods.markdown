---
title:  "Avoiding boxing in generic methods"
date:   2015-06-15 12:00:00
tags: .net boxing linq expressions
layout: post
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve --watch`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight csharp %}
private IDataSeries SetSciSeries(Chart2DSeries series, IRenderableSeries lineSeries)
{
	if (!Settings.YAxis.ScaleType.IsOneOf(AxisScaleType.Numerical, AxisScaleType.Auto) || !Settings.SecondaryYAxis.ScaleType.IsOneOf(AxisScaleType.Numerical, AxisScaleType.Auto))
	{
		throw new NotSupportedException();
	}

	switch (series.Settings.XScaleType)
	{
		case AxisScaleType.DateTime:
		{
			var xyDataSeries = new XyDataSeries<DateTime, double>();
			xyDataSeries.Append(series.Points.Select(d => (DateTime) d.X), series.Points.Select(d => (double) d.Y));
			lineSeries.DataSeries = xyDataSeries;
			return xyDataSeries;
			break;
		}
		case AxisScaleType.Numerical:
		case AxisScaleType.Auto:
		{
			var xyDataSeries = new XyDataSeries<double, double>();
			xyDataSeries.Append(series.Points.Select(d => (double) d.X), series.Points.Select(d => (double) d.Y));
			return xyDataSeries;
			break;
		}
		default:
			throw new NotSupportedException();
	}
}
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
