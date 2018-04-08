---
title: JS递归生成树形结构数据以及数据库遍历树型结构数据
date: 2018-04-08 16:02:44
categories:
- 前端
tags:
- JS
- 数据库
- 遍历

---

## 原始数据生成树型结构数据：


```
//递归正常数据，生成树形结构
export function getTreeNode({data,superId}){
    try {
        let treeNode=[];
        let temp=[];
        for(let i=0;i<data.length;i++){
            if(data[i].superId==superId){
                let obj=data[i];
                temp=getTreeNode({data,superId:data[i].id});
                if (temp.length > 0) {
                     obj.children = _.cloneDeep(temp);
                }
                treeNode.push(obj);
            }
        }
        return treeNode;
    } catch (e) {
        console.log("eeeeeeeee=>",e);
    }
}

```

### 复杂SQL，子查询以及递归遍历树形结构数据，多表连接


```
SELECT IFP.ID,
       IFP.SUPER_ID,
       IFP.PROTO_CODE,
       IFP.ACC_CODE,
       JSZD.JSZHZD_MC ZHMC,
       JSZD.JSZHZD_WBBH CURCD,
       JSZD.JSZHZD_KHJBRQ KHRQ,
       IFP.VALID_DATE,
       IFP.END_DATE,
       (SELECT IFPC.OPERATOR_DATE
          FROM IFP_FUNDSPOOL_CHANGE IFPC
         WHERE IFPC.MOD_TYPE = '1'
           AND IFPC.FUNDSPOOL_PROTO_ID = IFP.PROTO_CODE) RCRQ,
       (SELECT IFPC.OPERATOR_DATE
          FROM IFP_FUNDSPOOL_CHANGE IFPC
         WHERE IFPC.MOD_TYPE = '2'
           AND IFPC.FUNDSPOOL_PROTO_ID = IFP.PROTO_CODE) TCRQ,
       IFP.KEEP_METHOD,
       DECODE(IFP.KEEP_METHOD, '0', IFP.KEEP_RATE， '1', IFP.KEEP_LIMIT, 0) LC,
       IFP.PROXY_INT_FLAG,
       (SELECT NVL(A.JSZHRYEB_DQYE, 0) - NVL(A.JSZHRYEB_DJJE, 0)
          FROM JSZHRYEB A
         WHERE 1 = 1
           AND A.JSZHRYEB_RQ = '20171223'
           AND A.JSZHRYEB_BH = IFP.ACC_CODE) AVAI_AMT,
       (SELECT NVL(SUM(NVL(JEB.JSZHRYEB_SCYE, 0)), 0)
          FROM JSZHRYEB JEB
          LEFT JOIN IFP_FUNDSPOOL POO
            ON JEB.JSZHRYEB_BH = POO.ACC_CODE
         START WITH POO.SUPER_ID = IFP.ID
        CONNECT BY POO.SUPER_ID = PRIOR POO.ID
               AND JEB.JSZHRYEB_RQ = '20171223') GJJE,
       (SELECT COUNT(1)
          FROM IFP_FUNDSPOOL POO
         START WITH POO.SUPER_ID = IFP.ID
        CONNECT BY POO.SUPER_ID = PRIOR POO.ID) ACC_COUNT
  FROM IFP_FUNDSPOOL IFP
  LEFT JOIN JSZHZD JSZD
    ON JSZD.JSZHZD_BH = IFP.ACC_CODE
 WHERE 1 = 1
 START WITH IFP.ID IN (SELECT B.ID
                         FROM IFP_FUNDSPOOL B
                        WHERE 1 = 1
                          AND B.BRANCH_CODE = '99'
                          AND B.CUST_CODE = '22101003063')
CONNECT BY IFP.SUPER_ID = PRIOR IFP.ID

```

>之所以在查询中枚举出每个字段的值而不用select * 是因为枚举能提高数据库查询的速度，中间少去了列转化的时间。

