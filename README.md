# 预计支持的功能

- [x] Emoji 表情插入到输入框中显示
- [x] @成员弹出，以及在输入框高亮提示
- [x] 多会话，输入框内存缓存
- [ ] 支持图片插入，拖拽到输入框中显示
- [ ] 支持文件插入，拖拽到输入框中显示

# 地址

-[git地址](https://github.com/shenqil/react-im-input)
-[演示地址](https://shenqil.github.io/react-im-input/)

***

# 安装

```
npm i @shen9401/react-im-input pinyin-match 
```

- **@成员**支持模糊搜索，依赖`pinyin-match`

***

# 显示效果

![image.png](https://upload-images.jianshu.io/upload_images/25820166-54b45281248e9765.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
***

# 使用

```
import ImInput from '@shen9401/react-im-input'

function App(){
  const [out,setOut] = useState<EMsgItem[]>([])
  const imInputRef = useRef<IIMRef>(null)
  const memberList = useMemberList()

  function sendMsg(list:EMsgItem[]){
    setOut(list)
  }

  function handleEmojiClick(item:IEmojiItem){
    imInputRef.current?.insertEmoji(item)
  }

  return (
    <div className='example'>

      <div className='example_tools'>
        <Emoji handleEmojiClick={handleEmojiClick}/>
      </div>

      <div className='example_input'>
        <IMInput 
          memberList={memberList as IMemberItem[]} 
          handleSend={sendMsg}  
          onRef={imInputRef}
        />
      </div>

      <div className="example_btn">
          <div
            className="example_btn--inner"
            aria-hidden="true"
            onClick={()=>imInputRef.current?.sendMsg()}
          >
            发送
          </div>
      </div>

      <ul className='example_out'>
        {out.map((item,index)=>
        (<li key={index}>
          {JSON.stringify(item)}
          </li>)
        )}
      </ul>
    </div>
    )
}

```

***

## 1.Emoji 功能

- 直接调用内部方法 `imInputRef.current?.insertEmoji(item)`;item 满足 `{ key:string, data:base64 }`结构

## 2.@成员功能

- props 传入 `memberList` 满足 `[{  id:string,name:string, avatar:string}]`结构

## 3.发送消息

- props 传入`sendMsg`,按Enter键，或者调用`imInputRef.current?.sendMsg()`,会触发`sendMsg`回调

## 4.直接操作InnerHTML

- `imInputRef.current?.setInnerHTML` 直接覆盖输入框的内容
- `imInputRef.current?.getInnerHTML` 获取输入框的原生内容

## 5.多会话，输入框内容缓存

```js
import {clearCache} from '@shen9401/react-im-input'
```

- 切换`props`上的`inputId`,会缓存上一次id的输入框内容，从而回到上一次`inputId`时，内容能还原
- 清空指定`inputId`的缓存，`clearCache('inputId')`
- 清空所有缓存,`clearCache(undefined)`
