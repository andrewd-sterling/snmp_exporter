# Exporter file format

This is generated by the generator, so only those doing development should
have to care about how this works.

```
module_name:
  # There's various auth/version options here too. See the main README.
  walk:
    # List of OID subtrees to walk.
    - 1.3.6.1.2.1.1.3
    - 1.3.6.1.2.1.2
  metrics:      # List of metrics to extract.
     # A simple metric with no labels.
   - name:  sysUpTime
     oid:   1.3.6.1.2.1.1.3
     type:  gauge
     # Possible types are:
     #   gauge:   An integer with type gauge.
     #   counter: An integer with type counter.
     #   OctetString: A bit string, rendered as 0xff34.
     #   DisplayString: An ASCII string.
     #   PhysAddress48: A 48 bit MAC address, rendered as 00:01:02:03:04:ff.
     # Non-numeric types are represented as a gauge with value 1, and the rendered value
     # as a label value on that gauge.

     # A metric that's part of a table, and thus has labels.
   - name:  ifMtu
     oid:   1.3.6.1.2.1.2.2.1.4
     type:  gauge
     # A list of the table indexes and their types. All indexes become labels.
     indexes:
      - labelname: ifIndex
        type: gauge
   - name:  ifSpeed
     oid:   1.3.6.1.2.1.2.2.1.5
     type:  gauge
     indexes:
      - labelname: ifDescr
        type: gauge
     # Lookups take original indexes, look them up in another part of the
     # oid tree and overwrite the given output label.
     lookups:
       - labels: [ifDescr]         # Input label name(s).
         oid: 1.3.6.1.2.1.2.2.1.2  # OID to look under.
         labelname: ifDescr        # Output label name.
         type: OctetString         # Type of output object.
     # Creates new metrics based on the regex and the metric value.
     regex_extracts:
       Temp: # A new metric will be created appending this to the metricName to become metricNameTemp.
         - regex: '(.*)' # Regex to extract a value from the returned SNMP walks's value.
           value: '$1' # Parsed as float64, defaults to $1.
```
