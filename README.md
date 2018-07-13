# Glide Utils

This is a scoped application for Service-now to allow some utilities to be easily added and maintained on instances.

---

## Features

- Rewrite Journal content
- GlideRecord GlideAjax Helper
- Variable Helper

### Rewrite Journal Content

This adds a UI Action for admins to replace text on `task` for users with
the `admin` role.  This is what the feature looks like in action;
![](screenshot.gif)

### GlideRecord GlideAjax Helper aka WeeRow

This is my attempt at recreating a lighter GlideRecord called weeRow;

To use this you can call it via GlideAjax but because it's quite dynamic
you should not need to make other custom script includes in the future;

Example code;

```js
var queryObj = {
    table: 'sys_user',
    query: 'managerISNOTEMPTY',
    fields:['user_name','manager','phone']
};
var ga = new GlideAjax('x_8821_glide_utils.GlideUtils');
ga.addParam('sysparm_name', 'weeRow');
ga.addParam('sysparm_obj', JSON.stringify(queryObj));
ga.getXML(function(response) {
    var answer = response.responseXML.documentElement.getAttribute('answer');
    var serverObj = JSON.parse(answer);
    console.log(answer);
});
/*
{
  "status": "initialized",
  "table": "sys_user",
  "query": "managerISNOTEMPTY",
  "rows": [
    {
      "user_name": {
        "value": "melinda.carleton",
        "display": "melinda.carleton"
      },
      "manager": {
        "value": "02826bf03710200044e0bfc8bcbe5d3f",
        "display": "Lucius Bagnoli"
      },
      "phone": {
        "value": null,
        "display": ""
      }
    }
  ]
}
*/
```

### Variable Helper

This script include makes it easier to get the variables on
catalog items, their tasks, and other records;

```js
//example mail script
(function runMailScript(current, template, email, email_action, event) {
    template.print('<span style="font-size: 10pt; font-family: arial, geneva;">');
    template.print("<B>Additional details</B>:<br />"); 
    printVars();
    function printVars(){
        var vh = new x_8821_glide_utils.variableHelper();
        var vars = vh.getVariables(current);
        for (var i = 0; i < vars.length; i++) {
            template.space(6);
            template.print(' ' + "<b>" + vars[i].label + ": " + "</b>" + vars[i].value + "\n" + "<br/>");
        }
    }
})(current, template, email, email_action, event);
```

---



## Setup

1. Open Studio on your environment
1. Import from source
1. Paste in the following url: `https://gitlab.com/jacebenson/servicenow-glideutils.git`

---

## Usage

After you import this you can start to use it by in the following ways;

- Pull up an task, you with admin role, will see a "Replace Text" UI Action that works.