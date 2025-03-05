# 树形结构怎么用redis 做缓存


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# 树形结构怎么用redis 做缓存

### 1、树形结构的数据存储设计

树形结构通常包含父子节点关系，建议采用 **哈希表（Hash） + 有序集合（ZSet）** 组合存储：

1. **节点详情存储**
   每个节点以哈希表形式存储，键为 `node:{id}`，字段包含 `id`、`name`、`parentId` 等属性：

   ```java
   redisTemplate.opsForHash().put("node:1", "name", "根节点");
   redisTemplate.opsForHash().put("node:1", "parentId", "0");
   ```

2. **父子关系维护**
   使用有序集合（ZSet）存储子节点列表，键为 `children:{parentId}`，成员为子节点 ID，分数用于排序：

   ```java
   redisTemplate.opsForZSet().add("children:0", "1", 1); // 根节点的子节点
   redisTemplate.opsForZSet().add("children:1", "2", 1); // 子节点2的父节点是1
   ```

------

### 2、缓存逻辑实现

1. **缓存树节点查询**
   使用 `@Cacheable` 注解缓存单个节点查询结果，避免频繁访问数据库：

   ```java
   @Service
   public class TreeNodeService {
       @Cacheable(value = "nodes", key = "#id")
       public TreeNode getNodeById(String id) {
           // 模拟数据库查询
           return buildTreeNode(id);
       }
   }
   ```

   

2. **缓存子树或全树**
   对于高频访问的子树，可将序列化后的 JSON 结构存入 Redis 字符串类型：

   ```java
   @Cacheable(value = "tree", key = "#rootId")
   public String getSubTree(String rootId) {
       TreeNode root = getNodeById(rootId);
       // 递归构建子树并转为JSON
       return JSON.toJSONString(root);
   }
   ```

3. **缓存更新与删除**
   节点变更时，通过 `@CacheEvict` 清除相关缓存：

   ```java
   @CacheEvict(value = {"nodes", "tree"}, key = "#node.id")
   public void updateNode(TreeNode node) {
       // 更新数据库并同步更新Redis中的哈希表和子节点集合
   }
   ```

------

### 3、高级优化策略

1. **缓存穿透处理**
   对空结果设置短时间的缓存（如 5 分钟），避免频繁查询不存在的节点
2. **缓存雪崩预防**
   为不同节点设置随机过期时间，分散缓存失效时间点
3. **分布式场景一致性**
   使用 Redis 发布订阅（Pub/Sub）机制，在节点更新时通知其他服务实例清除本地缓存

------

### 4、示例代码片段

```java
// 获取子树（带缓存）
@Cacheable(value = "subtree", key = "#rootId")
public List<TreeNode> getSubTree(String rootId) {
    List<String> childIds = redisTemplate.opsForZSet()
        .range("children:" + rootId, 0, -1);
    return childIds.stream()
        .map(id -> {
            TreeNode node = getNodeById(id); // 触发节点缓存
            node.setChildren(getSubTree(id)); // 递归获取子节点
            return node;
        }).collect(Collectors.toList());
}
```


