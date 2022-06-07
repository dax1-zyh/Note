## 方法一

```javascript
<Tooltip placement="bottom">
    <Button>Multiple lines</Button>
    <div slot="content" style="white-space: normal;">
       A balloon appears when the mouse passes over this text
     </div>
</Tooltip>
```

## 方法二

```javascript
render: (h, params) => {
            let texts = ''; //表格列显示文字
            if (params.row.content !== null) {
              if (params.row.content.length > 6) { //进行截取列显示字数
                texts = params.row.content.substring(0, 6) + ".....";
              } else {
                texts = params.row.content;
              }
            }
            return h('div', [
              h('Tooltip', {
                props: {
                  placement: 'bottom',
                  // transfer: true  //是否将弹层放置于 body 内
                },
                style: {
                  cursor: 'pointer',
                }
              }, [ //这个中括号表示是Tooltip标签的子标签
                texts, //表格列显示文字
                h('div', {
                    slot: 'content',
                    style: {
                      whiteSpace: 'normal'
                    }
                  }, params.row.content //整个的信息即气泡内文字
                )
              ])
            ]);
          }
```