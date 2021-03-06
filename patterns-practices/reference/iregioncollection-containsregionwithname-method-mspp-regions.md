---
TOCTitle: ContainsRegionWithName Method
Title: 'IRegionCollection.ContainsRegionWithName Method (Microsoft.Practices.Prism.Regions)'
ms:assetid: 'M:Microsoft.Practices.Prism.Regions.IRegionCollection.ContainsRegionWithName(System.String)'
ms:mtpsurl: 'iregioncollection-containsregionwithname-method-mspp-regions.md'
---

# IRegionCollection.ContainsRegionWithName Method

Checks if the collection contains a [IRegion](/patterns-practices/reference/iregion-interface-mspp-regions) with the name received as parameter.

**Namespace:** [Microsoft.Practices.Prism.Regions](/patterns-practices/reference/mspp-regions-namespace)  
**Assembly:** Microsoft.Practices.Prism.Composition (in Microsoft.Practices.Prism.Composition.dll)  
**Version:** 5.0.0.0 (5.0.0.0)

## Syntax

```C#
bool ContainsRegionWithName(
	string regionName
)
```

### Parameters

*regionName*  
Type: [System.String](http://msdn.microsoft.com/en-us/library/s1wwdcbf)  
The name of the region to look for.

### Return Value  
Type: [Boolean](http://msdn.microsoft.com/en-us/library/a28wyd50)  
**truetrue** (**True** in Visual Basic) if the region is contained in the collection, otherwise **falsefalse** (**False** in Visual Basic).

```VB
'Declaration
Function ContainsRegionWithName ( 
	regionName As String
) As Boolean
```

### Parameters

*regionName*  
Type: [System.String](http://msdn.microsoft.com/en-us/library/s1wwdcbf)  
The name of the region to look for.

### Return Value  
Type: [Boolean](http://msdn.microsoft.com/en-us/library/a28wyd50)  
**Truetrue** (**True** in Visual Basic) if the region is contained in the collection, otherwise **Falsefalse** (**False** in Visual Basic).

## See Also

[IRegionCollection Interface](/patterns-practices/reference/iregioncollection-interface-mspp-regions)  
[IRegionCollection Members](/patterns-practices/reference/iregioncollection-members-mspp-regions)  
[Microsoft.Practices.Prism.Regions Namespace](/patterns-practices/reference/mspp-regions-namespace)