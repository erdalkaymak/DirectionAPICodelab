---
title: Additional Information
description: 1
---

<p>This project has been developed with MVP design pattern. Please check the interface, presenter and activity classes are generated for each feature. In interface classes, presenter and view interfaces are defined. With the presenter object that initialized in activity classes, the methods will be called where actual work has been done to use Huawei Map SDK with Direction API capabilities. In this way, logic dependency will be removed from activity classes. These methods are generated inside presenter classes.</p>

<p>In presenter classes, view objects have been defined. Override methods will be called inside presenter classes through view objects to take user actions. For example, when user choose walking tab steps are showed, this steps are obtained inside presenter class with using view object. In other words, presenter provides the desired results for UI while view is taking the user action.</p>

<p>This architecture enables to provide maintainable and adaptable design for users. You can build up your app based on it and use the demo to shape your usage of Map Kit.</p>
  

