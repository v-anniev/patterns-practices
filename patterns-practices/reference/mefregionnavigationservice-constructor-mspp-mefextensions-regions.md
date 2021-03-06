---
TOCTitle: MefRegionNavigationService Constructor
Title: 'MefRegionNavigationService Constructor (Microsoft.Practices.Prism.MefExtensions.Regions)'
ms:assetid: 'M:Microsoft.Practices.Prism.MefExtensions.Regions.MefRegionNavigationService.\#ctor(Microsoft.Practices.ServiceLocation.IServiceLocator,Microsoft.Practices.Prism.Regions.IRegionNavigationContentLoader,Microsoft.Practices.Prism.Regions.IRegionNavigationJournal)'
ms:mtpsurl: 'mefregionnavigationservice-constructor-mspp-mefextensions-regions.md'
---

# MefRegionNavigationService Constructor

Initializes a new instance of the [MefRegionNavigationService](/patterns-practices/reference/mefregionnavigationservice-class-mspp-mefextensions-regions) class.

**Namespace:** [Microsoft.Practices.Prism.MefExtensions.Regions](/patterns-practices/reference/mspp-mefextensions-regions-namespace)  
**Assembly:** Microsoft.Practices.Prism.MefExtensions (in Microsoft.Practices.Prism.MefExtensions.dll)  
**Version:** 5.0.0.0 (5.0.0.0)

## Syntax

```C#
public MefRegionNavigationService(
	IServiceLocator serviceLocator,
	IRegionNavigationContentLoader navigationContentLoader,
	IRegionNavigationJournal journal
)
```

```VB
'Declaration
Public Sub New ( 
	serviceLocator As IServiceLocator,
	navigationContentLoader As IRegionNavigationContentLoader,
	journal As IRegionNavigationJournal
)
```

### Parameters

*serviceLocator*   
Type: IServiceLocator   
The service locator.

*navigationContentLoader*  
Type: [Microsoft.Practices.Prism.Regions.IRegionNavigationContentLoader](/patterns-practices/reference/iregionnavigationcontentloader-interface-mspp-regions)   
The navigation content loader.

*journal*   
Type: [Microsoft.Practices.Prism.Regions.IRegionNavigationJournal](/patterns-practices/reference/iregionnavigationjournal-interface-mspp-regions)   
The navigation journal.

## See Also

[MefRegionNavigationService Class](/patterns-practices/reference/mefregionnavigationservice-class-mspp-mefextensions-regions)  
[MefRegionNavigationService Members](/patterns-practices/reference/mefregionnavigationservice-members-mspp-mefextensions-regions)  
[Microsoft.Practices.Prism.MefExtensions.Regions Namespace](/patterns-practices/reference/mspp-mefextensions-regions-namespace)  