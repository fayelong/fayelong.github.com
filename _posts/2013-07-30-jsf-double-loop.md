---
layout: post
title: "JSF遍历双重循环"
description: "JSF遍历双重循环"
category: JSP
tags: [JSP]
---
{% include JB/setup %}

在jsf页面中经常使用c:foreach或者t:datalist对List或者Map进行循环遍历，但一般是遍历一层。现在需要对其进行双重循环。

## 对List<List<Bean>>进行遍历 ##

前提：

	{% highlight java %}
    List<List<ServeStationBean>> beanlists;
	{% endhighlight %}

方案一：使用t:dataList

步骤：

	1.在jsp页面添加t标签说明：
	
	{% highlight html %}
	<%@ taglib uri="http://myfaces.apache.org/tomahawk" prefix="t"%>
	{% endhighlight %}

	2.body中按如下方法进行遍历：
	
	{% highlight html %}
	<body>
	<f:view>
		<h:form>
			<t:dataList value="#{ServeStationBean.beanlists }" var="item1">
				<t:column>
					<div id="mapwebsite">
						<div class="title">
							<h:outputText value="#{item1.get(0).description }"></h:outputText>
						</div>
						<div class="content">
							<t:dataList value="#{item1 }" var="item2">
								<t:column>
									<div class="stationmap">
										<h:commandLink actionListener="#{showContent.check }"
											action="#{showContent.action }" value="#{item2.title }">
											<f:param name="uuid" value="#{item2.node_uuid }" />
											<f:param name="path" value="#{item2.path }" />
										</h:commandLink>
									</div>
								</t:column>
							</t:dataList>
						</div>
					</div>
				</t:column>
			</t:dataList>
		</h:form>
	</f:view>
	</body>
	{% endhighlight %}

方案二：使用c:forEach

步骤：

	1.在jsp页面添加c标签说明：
	
	{% highlight html %}
	<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
	{% endhighlight %}

	2.body中按如下方法进行遍历：

	{% highlight html %}
	<c:forEach items="${ ServeStationBean.beanlists }" var="item1">
		<div id="mapwebsite">
			<div class="title"> 
				${item1.get(0).description }
			</div>
			<div class="content">
				<c:forEach items="${item1 }" var="item2">
					<div class="stationmap">
						${item2.title }
					</div>
				</c:forEach>
			</div>
		</div>
	</c:forEach>
	{% endhighlight %}
	
## 对Map<List<Bean>>进行遍历 ##

前提：
	
	{% highlight java %}
	Map<String, List<ServeStationBean>> beanmap;
	{% endhighlight %}

方案：使用c:forEach

	1.在jsp页面添加c标签说明：
	
	{% highlight html %}
	<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
	{% endhighlight %}

	2.body中按如下方法进行遍历：

	{% highlight html %}
	<c:forEach items="${ ServeStationBean.beanmap }" var="map">
		<div id="mapwebsite">
			<div class="title">
				${map.key }
			</div>
			<div class="content">
				<c:forEach items="${map.value}" var="list">
					<div class = "stationmap">
						${list.title }
					</div>
				</c:forEach>
			</div>
		</div>
	</c:forEach>
	{% endhighlight %}

t:dataList不支持Map遍历，因此只能使用c:forEach