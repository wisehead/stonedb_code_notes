#1.Engine::InsertMemRow

```
Engine::InsertMemRow
--EncodeRecord(table_path, share->TabID(), table->field, table->s->fields, table->s->blob_fields, buf, buf_sz);
--rctable = share->GetSnapshot();
----current = std::make_shared<RCTable>(table_path, this);
--rctable->InsertMemRow(std::move(buf), buf_sz);

```