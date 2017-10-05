---
title: Kendo ComboBox Virtualization with MVVM
description: An example on how to implement virtualization for ComboBox
type: how-to
page_title: Implement Virtualization for MVVM ComboBox | Kendo UI ComboBox
slug: how-to-virtualize-mvvm-combobox
position: 0
tags: kendoui, kendo, kendo, kendoui, combobox, mvvm, virtualization
teampulseid:
ticketid: 1121707
pitsid:
res_type: kb
---

## Environment

<table>
 <tr>
  <td>Product</td>
  <td>MVVM for Progress® Kendo UI®</td>
 </tr>
 <tr>
  <td>Operating System</td>
  <td>Windows 10 64bit</td>
 </tr>
 <tr>
  <td>Browser</td>
  <td>All</td>
 </tr>
</table>


## Description

How can I implement virtualization for my ComboBox in MVVM binding scenario?

## Solution

You should specify the itemHeight and the valueMapper in the **data-virtual** attribute of the combobox:

``` html 

<div id="example">
        <h4>Search for shipping name</h4>
        <input id="orders" style="width: 400px"
                data-role="combobox"
                data-bind="value: order, source: source"
                data-text-field="ShipName"
                data-value-field="OrderID"
                data-filter="contains"
                data-virtual="{itemHeight:26,valueMapper:orderValueMapper}"
                data-height="520"/>
</div>
<script>
$(document).ready(function() {
        var model = kendo.observable({
        source: new kendo.data.DataSource({
                type: "odata",
                transport: {
                        read: "http://demos.telerik.com/kendo-ui/service/Northwind.svc/Orders"
                },
                schema: {
                        model: {
                        fields: {
                                OrderID: { type: "number" },
                                Freight: { type: "number" },
                                ShipName: { type: "string" },
                                OrderDate: { type: "date" },
                                ShipCity: { type: "string" }
                                }
                        }
                },
                pageSize: 80,
                serverPaging: true,
                serverFiltering: true
                })
        });

        kendo.bind($(document.body), model);
});

function orderValueMapper(options) {
        $.ajax({
                url: "http://demos.telerik.com/kendo-ui/service/Orders/ValueMapper",
                type: "GET",
                dataType: "jsonp",
                data: convertValues(options.value),
                success: function (data) {
                options.success(data);
                }
        })
}

function convertValues(value) {
        var data = {};

        value = $.isArray(value) ? value : [value];

        for (var idx = 0; idx < value.length; idx++) {
        data["values[" + idx + "]"] = value[idx];
        }

        return data;
}
</script>

```

## See Also

Combobox Virtualization: [http://docs.telerik.com/kendo-ui/controls/editors/combobox/virtualization#data-and-ui-virtualization](ttp://docs.telerik.com/kendo-ui/controls/editors/combobox/virtualization#data-and-ui-virtualization)