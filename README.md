# test-react-projects
Тестирование реакт приложений
1. В местах рендеринга styled-components надо импортировать бииблиотеку - import 'jest-styled-components'; Таким образом в итоговый снэпшот у Вас будут попалать не только названия сгенерированных классов, но и сненерированный css(особенно это помогает когда внутри styled-components есть логика, без импорта этой библиотеки у Вас ничего не будет выведено в снепшот)
Пример:

<code>
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
    <div
      className="sc-gtsrHT iYboLY"
    />
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
</code>



