#1.class RCAttr

```cpp
/*
● stonedb底层存储分为PackInt和PackStr两种类型，其中涉及到精度损失的有两部分：一部分index编码中，一部分在PackInt转码存储到Data中。这两部分可能都需要更改。
● RCAttr是PhysicalColumn的实现类，可实现加载data pack，并转换成对应的PackInt以及PackStr基础存储类型。
● 查询时要创建RCTable的包装类TempTable，并包装了attr，filter以及multiindex，实现过滤以及内存加载增强功能。
● PackInt编码存储到Data文件时，为啥要减去一个dpn.min_i，怕溢出？
*/
class RCAttr final : public mm::TraceableObject, public PhysicalColumn, public PackAllocator {
  friend class RCTable;
 private:
  COL_VER_HDR hdr{};
  common::TX_ID m_version;  // the read-from version
  Transaction *m_tx;
  int m_tid;
  int m_cid;
  ColumnShare *m_share;

  std::shared_ptr<FTree> m_dict;
  std::vector<common::PACK_INDEX> m_idx;

  bool no_change = true;

  // local filters for write session
  std::shared_ptr<RSIndex_Hist> filter_hist;
  std::shared_ptr<RSIndex_CMap> filter_cmap;
  std::shared_ptr<RSIndex_Bloom> filter_bloom;

  std::shared_ptr<RSIndex_Hist> GetFilter_Hist();
  std::shared_ptr<RSIndex_CMap> GetFilter_CMap();
  std::shared_ptr<RSIndex_Bloom> GetFilter_Bloom();
  uint8_t pss;
  common::PackType pack_type;
  double rough_selectivity = -1;  // a probability that simple condition "c = 100" needs to open a data
                                  // pack, providing KNs etc.
  std::function<std::shared_ptr<RSIndex>(const FilterCoordinate &co)> filter_creator;
};  
```