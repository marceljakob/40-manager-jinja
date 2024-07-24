## Purpose
This page is intended to provide a more in-depth guideline regarding the Jinja integration in FortiManager.
It is intended to help colleagues, customers and partners to collect all necessary information on a single page und provide a proper enablement.
The repository a privately maintained guideline and has no association to the official fortinet documentation.
Furthermore, realistic examples from customer use-cases are shown and explained, which can provide a deeper insight into the Jinja configuration.


## Support
The following utilities are supported on FortiManager:
- All of Jinja 2.11 operations, built-in filters and tests.
- Ansible's ipaddr filters.
- Python 3.8's built-in functions.
- Jinja’s import and include.


## Important Considerations
### Whitespace Control
FortiManager uses the default Jinja configuration for whitespace control:
- A single trailing newline is stripped if present.
- Other whitespace (spaces, tabs, and newlines) are returned unchanged.
Therefore it is very important to manually strip whitespace characters. A minus sign (-) can be added to the start or end of a block, a comment, or a variable expression to remove the whitespace before or after the block.
**Experience has shown that most errors within the generated CLI script arises from whitespace controll misconfigurations.** 


## Snipplets
### Hostname
### Interfaces
### SDWAN Member Configuration
XXX: Create and arrange the interface members via the call of macro function based on the corresponding input.
```
{%- macro conn(var1, var2, var3, var4) -%}
{%- set array = [ var1, var2, var3, var4] -%}

{%- set members = "" -%} 

{%- for item in array -%}
{%- if (item== 'MPLS') -%} 
{%- set members = members ~ " 3" -%}
{%- endif -%}
{%- if item == 'ISP1'  -%}
{%- set members = members ~ " 11 21" -%}
{%- endif -%}
{%- if item == 'ISP2' -%} 
{%- set members = members ~ " 12 22" -%}
{%- endif -%}
{%- if item == 'LTE' -%} 
{%- set members = members ~ " 13 23" -%}
{%- endif -%}
{{- members -}}
{%- endfor -%}
{%- endmacro -%}
-----

set members {{ conn('ISP1', 'ISP2', 'LTE', 'MPLS')}}
**Output:** set members  11 21 12 22 13 23 3

set members {{ conn('ISP1', '', '', '')}}
**Output:** set members  11 21
```

## Links
Offical Fortinet Documentation: [Jinja Filters and Functions](https://docs.fortinet.com/document/fortimanager/7.4.1/jinja-filters-and-functions/130068/supported-filters-and-functions)

Offical Fortinet Documentation: [Jinja2 template sample scripts](https://docs.fortinet.com/document/fortimanager/7.2.0/new-features/761880/jinja2-template-sample-scripts)

Offical Fortinet Documentation: [FortiManager meta variables in Jinja](https://docs.fortinet.com/document/fortimanager/7.4.1/jinja-filters-and-functions/456481/fortimanager-meta-variables-in-jinja)

## notes
Set Hostname
Set interfaces with check if ports (meta) are definied with IP address
set member and reihenfolge via macro
