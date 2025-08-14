# 🗎 XML — Extensible Markup Language

XML (**Extensible Markup Language**) is a flexible, text-based format for storing and transporting structured data.  
It is both **human-readable** and **machine-readable**, making it widely used for configuration files, data exchange, and document storage.

---

## 📚 Contents

- [XML Basics](#xml-basics)

---

## XML Basics

### Syntax Rules

XML is strict about how you write it. If you break these rules, a parser will fail.

- **All tags must be closed**

```xml
<book>The Fellowship of the Ring</book>     ✅
<book>The Fellowship of the Ring            ❌

```

- **Proper Nesting**

```xml
<parent>
    <child>Data</child>                 ✅
</parent>


<parent>
    <child>Data</parent></child>        ❌

```

- **Case-sensitive**

```xml
<book>The Fellowship of the Ring</book>     ✅
<book>The Fellowship of the Ring</Book>     ❌
```

- **One root element per XML document**

```xml
<books>...</books>  <!-- Only one outermost element -->

```

- **Attribute values must be quoted**

```xml
<book title="The Fellowship of the Ring"/>  ✅
<book title=The Fellowship of the Ring/>    ❌

```

- **Tag and attribute name rules**
  - Must start with a letter or underscore
  - Can contain letters, digits, hyphens and periods
  - Colon `:` is reserved for namespaces

### Elements and Attributes

### XML Declaration

### Nesting and Well-Formatted Documents

### Comments and CDATA
