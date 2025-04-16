## develop interface

```
const [value, setValue] = useState('')

<AutoComplete options={[...totalOptions]}>
  <AutoCompleteInput value={value} onChange={(e)=>setValue(e.target.value)}/>
  <AutoCompleteContent>
    {({list})=>list.map(item=><AutoCompleteItem value={item} />)}
  </AutoCompleteContent>
</AutoComplete>
```

## feature

1. input에 포커스가 생기면 AutoCompleteContent가 portal로 열린다.
2. input에 타이핑이 없으면 모든 options가 AutoCompleteContent에 랜더링된다.
3. input에 타이핑이 시작되면 options 중에 label값이 input value를 포함하는 것들이 필터링되어 보여진다.
4. 랜더링 된 AutoCompleteContent내부의 AutoCompleteItem을 클릭하면 해당 값이 선택된다.
5. input에 타이핑된 값이 포함되는 option이 없으면 포탈이 닫힌다.
6. 아래쪽이 portal 공간이 부족하면 portal이 위쪽으로 열린다. 둘다 부족하면 아래쪽으로 열린다.
7. portal에서 값이 필터링되어 item 갯수가 줄어들면 AutoCompleteInput에서 멀어지지 않고 붙어서 랜더링되어야 한다.
8. AutoCompleteItem을 클릭해서 선택할 시, input에는 options의 label이 채워진다.
9. clear 기능은 제외한다.

## sample-image

![AutoComplete Sample Image](./autocomplete-sample.png)
