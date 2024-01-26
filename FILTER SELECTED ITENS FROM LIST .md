## FILTER SELECTED ITENS FROM LIST 

#### HTML
```
<lightning-datatable
    key-field="id"
    data={data}
    columns={columns}
    selected-rows={selectedRows}
    onrowselection={handleRowSelection}
>
</lightning-datatable>


```
#### JS
```
import { LightningElement, track } from 'lwc';

export default class SuaComponente extends LightningElement {
    @track data = [
        { id: '1', name: 'Item 1' },
        { id: '2', name: 'Item 2' },
        { id: '3', name: 'Item 3' },
        // ... outros dados
    ];

    @track selectedRows = [];

    get columns() {
        return [
            { label: 'ID', fieldName: 'id' },
            { label: 'Name', fieldName: 'name' },
            // ... outras colunas
        ];
    }

    handleHideNonSelected() {
        // Obtenha IDs das linhas selecionadas
        const selectedIds = new Set(this.selectedRows.map(row => row.id));

        // Filtra os dados para manter apenas as linhas selecionadas
        this.data = this.data.filter(item => selectedIds.has(item.id));
    }

    handleRowSelection(event) {
        // Atualiza o array de linhas selecionadas quando a seleção é alterada
        this.selectedRows = event.detail.selectedRows;
    }
}

```
