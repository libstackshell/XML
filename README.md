# XML
XML functions

```XML
<?xml version="1.0" encoding="utf-8"?>
<CATALOG>
  <CDS>
    <CD id="1" country="USA">
      <TITLE>Empire Burlesque</TITLE>
      <ARTIST>Bob Dylan</ARTIST>
      <COMPANY>Columbia</COMPANY>
      <PRICE currency="EUR">10.90</PRICE>
      <YEAR>1985</YEAR>
    </CD>

    <CD id="2" country="UK">
      <TITLE>Hide your heart</TITLE>
      <ARTIST>Bonnie Tyler</ARTIST>
      <COMPANY>CBS Records</COMPANY>
      <PRICE currency="EUR">9.90</PRICE>
      <YEAR>1988</YEAR>
    </CD>
  </CDS>
</CATALOG>
```

```
function main() {

    var f = std.fopen("test.xml", "r");
    var data = std.fread(f);
    std.fclose(f);

    var doc = xml.parse(data);

    print("== xml.has ==");
    print("has /CATALOG/CDS/CD => " + xml.has(doc, "/CATALOG/CDS/CD") + "\n\n");

    print("== xml.select (Element) ==");
    var companyNode = xml.select(doc, "/CATALOG/CDS/CD[1]/COMPANY");
    print("is elem: " + xml.is_elem(companyNode) + "\n");
    print("value: " + xml.value(companyNode) + "\n\n");

    print("== xml.select (Attribute) ==");
    var idAttr = xml.select(doc, "/CATALOG/CDS/CD[1]/@id");
    print("is attr: " + xml.is_attr(idAttr) + "\n");
    print("attr value: " + xml.value(idAttr) + "\n\n");

    print("== xml.attr (Attribute by name) ==");
    var cd1 = xml.select(doc, "/CATALOG/CDS/CD[1]");
    print("cd1 id via xml.attr: " + xml.attr(cd1, "id") + "\n\n");

    print("== xml.set (Element value) ==");
    xml.set(doc, "/CATALOG/CDS/CD[1]/COMPANY", "NEW_LABEL");
    print("company after set: " + xml.value(xml.select(doc, "/CATALOG/CDS/CD[1]/COMPANY")) + "\n\n");

    print("== xml.set (Attribute value) ==");
    xml.set(doc, "/CATALOG/CDS/CD[1]/@id", "999");
    print("id after set: " + xml.value(xml.select(doc, "/CATALOG/CDS/CD[1]/@id")) + "\n\n");

    print("== xml.select_all (alle CD Nodes) ==");
    var cds = xml.select_all(doc, "/CATALOG/CDS/CD");
    print("select_all returned: " + cds + "\n\n"); // je nach ScriptStack wird das evtl. als ArrayList angezeigt

    print("== xml.iter (children von /CATALOG/CDS/CD[1]) ==");
    var it = xml.iter(cd1);
    while (xml.next(it)) {
        print("" + xml.index(it) + " name=" + xml.name(it) + " val=" + xml.value_it(it) + "\n");
    }
    print("\n");

    print("== xml.iter_attr (Attribute von CD[1]) ==");
    it = xml.iter_attr(cd1);
    while (xml.next(it)) {
        print(xml.name(it) + " = " + xml.value_it(it) + "\n");
    }
    print("\n");

    print("== xml.iter_all (Descendants unter /CATALOG) ==");
    var catalog = xml.select(doc, "/CATALOG");
    it = xml.iter_all(catalog);
    while (xml.next(it)) {
        // hier sind es nur Elemente (Descendants())
        print(""+ xml.index(it) + " <" + xml.name(it) + ">\n");
    }

    print("\n== Final XML (pretty)==");
    print(xml.string(doc));

}
```
