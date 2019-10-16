Muchas veces necesitamos que nuestro site o app sea multi-idioma

Realmente hay muchas formas de realizarlo, pero en este caso le voy a mostrar una solución muy sencilla

### Instalación
```bash
npm i @ngx-translate/core

npm i @ngx-translate/http-loader
```

### Estructura
```bash
├───app/
│   ├───app.component.css
│   ├───app.component.html
│   ├───app.component.ts
│   └───app.module.ts
├───assets/
│   ├───i18n/
│   │   ├───en.json
│   │   ├───es.json
│   │   ├───fr.json
│   │   └───it.json
```

### Modificar app.module.ts
```javascript
import {
  TranslateLoader,
  TranslateModule
} from '@ngx-translate/core';
import {
  TranslateHttpLoader
} from '@ngx-translate/http-loader';
import {
  HttpClient,
  HttpClientModule
} from '@angular/common/http';

@NgModule({
  imports: [
    HttpClientModule,
    TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: HttpLoaderFactory,
        deps: [HttpClient]
      }
    })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}

export function HttpLoaderFactory(http: HttpClient) {
  return new TranslateHttpLoader(http);
}
```

### Modificar app.component.ts
```javascript
import {
  Component
} from '@angular/core';
import {
  TranslateService
} from '@ngx-translate/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'translate';
  langList = ['es', 'en', 'fr', 'it'];

  constructor(private translate: TranslateService) {
    translate.setDefaultLang('es');
  }

  useLang(lang: string) {
    this.translate.use(lang);
  }
}
```

### Modificar app.component.html
```html
<div>
  <h2>{{'dummy.title' | translate}}</h2>
  <p>{{'dummy.text' | translate}}</p>
</div>

<select #formLang (change)='useLang(formLang.value)'>
  <option disabled>{{'dummy.lang' | translate}}</option>
  <option *ngFor='let item of langList' [value]="item">{{item}}</option>
</select>
```

### Crear diccionario
```json
{
  "dummy": {
      "title": "Traducción dummy",
      "text": "Esta es una aplicación de demostración simple para ngx-translate",
      "lang": "seleccione un idioma"
  }
}
```
