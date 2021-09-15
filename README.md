# test-react-projects
<h3> Тестирование реакт приложений</h3>
1. В местах рендеринга styled-components надо импортировать бииблиотеку - import 'jest-styled-components'; Таким образом в итоговый снэпшот у Вас будут попалать не только названия сгенерированных классов, но и сненерированный css(особенно это помогает когда внутри styled-components есть логика, без импорта этой библиотеки у Вас ничего не будет выведено в снепшот)
Пример:

#code
 <pre>
    const Row = styled.div`
        padding: 16px;
        height: 54px;
        cursor: pointer;
        display: flex;
        flex-direction: row;
        align-items: center;    &:hover {
            background: ${props => props.theme.colors.bgSecondary};
        }
    `;
    
    
    без библиотеки:
    < div className="sc-gtsrHT iYboLY"/>

    с библиотекой
    
    .c0 {
      padding: 16px;
      height: 54px;
      cursor: pointer;
      display: -webkit-box;
      display: -webkit-flex;
      display: -ms-flexbox;
      display: flex;
      -webkit-flex-direction: row;
      -ms-flex-direction: row;
      flex-direction: row;
      -webkit-align-items: center;
      -webkit-box-align: center;
      -ms-flex-align: center;
      align-items: center;
    }.c0:hover {
      background: #F5F5F5;
    }<div
      className="c0"
    />
    </pre>

2. Мокать все внешние компоненты в компоненты в jest.setup.ts, для того, чтобы не заниматься тестированием других библиотек
например если сейчас не мокать компоненты @example, то весь итоговый код этих компонентов попадет вам в снэпшот, а в случае изменения этих компонентов внешних библиотек, ваши снэпшоты связанные с этими библиотеками автоматически станут невалидными
в jest.setup.ts добавляется строка

<pre>jest.mock('@example/ui/components/Button', () => ({
        Button: createMockedComponent('Button'),
    }));
    
хэлпер для мока

const createMockedComponent = (componentName: string) =>
        class extends React.Component {
            public render() {
                return React.createElement(
                    componentName,
                    this.props,
                    this.props.children
                );
            }
        };
</pre>

итогом этого будет следующий снэпшот при рендере Button(если передать в баттон какие-либо пропы будет выведен только компонент и пропы)

<Button />

без мока ваш снэпшот будет выглядеть несколько больше
<pre>
c0 {
  border-radius: 8px;
  padding: 0;
  -webkit-text-decoration: none;
  text-decoration: none;
  position: relative;
  display: -webkit-inline-box;
  display: -webkit-inline-flex;
  display: -ms-inline-flexbox;
  display: inline-flex;
  -webkit-align-items: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: center;
  -webkit-justify-content: center;
  -ms-flex-pack: center;
  justify-content: center;
  cursor: pointer;
  width: auto;
  box-sizing: border-box;
  height: 48px;
  font-family: "SB Sans Text",Helvetica,Arial,sans-serif;
  font-weight: 600;
  font-size: 16px;
  line-height: 20px;
  -webkit-letter-spacing: -0.288px;
  -moz-letter-spacing: -0.288px;
  -ms-letter-spacing: -0.288px;
  letter-spacing: -0.288px;
  border: none;
  background-color: #0066FF;
  color: #FFFFFF;
  padding-left: 256px;
  padding-right: 256px;
}.c0 svg path {
  fill: currentColor;
}.c0:focus {
  outline: none;
}<button
  className="sc-gtsrHT c0"
  onBlur={[Function]}
  onClick={[Function]}
  onDragStart={[Function]}
  onFocus={[Function]}
  onKeyDown={[Function]}
  onKeyUp={[Function]}
  onMouseDown={[Function]}
  onMouseEnter={[Function]}
  onMouseLeave={[Function]}
  onMouseUp={[Function]}
  onTouchCancel={[Function]}
  onTouchEnd={[Function]}
  onTouchMove={[Function]}
  onTouchStart={[Function]}
/>
</pre>

3. есть еще третий вредный совет: чтобы автоматически отлавливать второй пункт. нужно убрать из транспайлинга в джест конфиге библиотеку. так если вы вдруг используете компонент из @example, который раньше не мокался глобально, то тест у вас упадет еще на стадии разработки, после чего вы сможете его добавить в список моков и все будет работать.
альтернатива1: написать сразу мок на все компоненты.
альтенатива2: попросить сделать эти моки сразу на все компоненты, чтобы всем проектам не пришлось их писать :)


