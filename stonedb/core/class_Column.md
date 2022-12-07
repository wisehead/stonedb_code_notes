#1.class Column

```cpp
/*! \brief Base class for columns.
 *
 * Defines the common interface for RCAttr, Attr and vcolumn::VirtualColumn
 */
class Column {
 public:
  Column(ColumnType ct = ColumnType()) : ct(ct) {}
  Column(const Column &c) {
    if (this != &c) *this = c;
  }

  inline const ColumnType &Type() const { return ct; }
  inline common::CT TypeName() const { return ct.GetTypeName(); }
  inline void SetTypeName(common::CT type) { ct.SetTypeName(type); }
  void SetCollation(DTCollation collation) { ct.SetCollation(collation); }
  DTCollation GetCollation() { return ct.GetCollation(); }

 protected:
  ColumnType ct;
};
```