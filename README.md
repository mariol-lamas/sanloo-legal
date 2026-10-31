# sanloo-legal

Sitio estático con la documentación legal de la app **Sanloo**: política de
privacidad, términos y condiciones, aviso de cookies y soporte.

Es solo front: HTML y una hoja de estilos. Sin build, sin dependencias, sin
JavaScript. Se sirve con GitHub Pages tal cual.

```
index.html          Portada con el resumen y los enlaces
privacidad.html     Política de privacidad (el documento principal)
terminos.html       Términos y condiciones
cookies.html        Aviso de cookies
soporte.html        Contacto, compras, borrado de cuenta y derechos
assets/styles.css   Estilos, con la paleta de la app y modo oscuro
.nojekyll           Desactiva Jekyll en GitHub Pages
```

## ⚠️ Antes de publicar: rellenar los datos del responsable

El contenido está completo, pero la identidad del responsable del
tratamiento está como marcador. **Una política de privacidad sin esos datos
no es válida** y App Store Connect la rechaza.

Hay cuatro marcadores:

| Marcador | Qué poner |
|---|---|
| `[NOMBRE Y APELLIDOS]` | Nombre completo del autónomo responsable |
| `[NIF]` | NIF |
| `[DIRECCIÓN POSTAL COMPLETA]` | Domicilio fiscal (calle, número, CP, ciudad) |
| `[EMAIL DE CONTACTO]` | Correo de privacidad y soporte |

Para localizarlos todos:

```sh
grep -rn "\[NOMBRE Y APELLIDOS\]\|\[NIF\]\|\[DIRECCIÓN POSTAL COMPLETA\]\|\[EMAIL DE CONTACTO\]" .
```

Para sustituirlos de golpe (macOS):

```sh
for f in *.html; do
  sed -i '' \
    -e 's/\[NOMBRE Y APELLIDOS\]/Nombre Apellido Apellido/g' \
    -e 's/\[NIF\]/00000000X/g' \
    -e 's/\[DIRECCIÓN POSTAL COMPLETA\]/Calle Ejemplo 1, 28001 Madrid/g' \
    -e 's/\[EMAIL DE CONTACTO\]/hola@sanloo.app/g' \
    "$f"
done
```

El correo aparece también en `mailto:`, así que el `sed` lo arregla en los
dos sitios de una pasada.

## Publicar con GitHub Pages

1. **Settings → Pages**.
2. *Source*: **Deploy from a branch**.
3. *Branch*: `main`, carpeta `/ (root)`. Guardar.

En un minuto queda servido en
`https://mariol-lamas.github.io/sanloo-legal/`.

URLs de cada documento:

| Documento | URL |
|---|---|
| Privacidad | `.../sanloo-legal/privacidad.html` |
| Términos | `.../sanloo-legal/terminos.html` |
| Cookies | `.../sanloo-legal/cookies.html` |
| Soporte | `.../sanloo-legal/soporte.html` |

### Con dominio propio

Si apuntas `sanloo.app` (o un subdominio como `legal.sanloo.app`) a GitHub
Pages, añade un archivo `CNAME` en la raíz con el dominio y configúralo en
Settings → Pages → Custom domain.

## Dónde se usan estas URLs

- **App Store Connect** → *Privacy Policy URL* y *Support URL*.
- **Google Play Console** → *Política de privacidad*. Play exige además una
  URL de eliminación de cuenta accesible sin instalar la app: sirve
  `privacidad.html#borrar` o `soporte.html`, que explican el procedimiento
  paso a paso.
- Dentro de la app, enlazados desde **Perfil → Privacidad y seguridad**.

## Mantener el contenido a la par que el código

El contenido no es genérico: describe el tratamiento real del backend. Si
cambias cualquiera de estas cosas, hay que tocar la política:

| Si cambias esto en el código | Revisa esto en la política |
|---|---|
| `RETENTION_DAYS` (`sanlo-back/app/core/config.py`) | Los 30 días del apartado 7 y del apartado 4 |
| El proveedor del chat o el despliegue de Azure OpenAI | Apartados 4 y 5 |
| `MIN_USER_AGE` (`sanlo-back/app/models/user.py`) | Apartados 3 y 10, y los términos |
| Qué se envía al modelo (`app/agents/sanloo_chat_agent.py`) | Apartado 4 |
| Lo que extrae `memory_service.py` | Apartado 4 |
| La región de Azure o de Supabase | Apartado 6 |
| Columnas nuevas con datos personales | Apartado 2 |
| Añadir un SDK de analítica o publicidad de terceros | Apartados 5 y 14, y el aviso de cookies |

Cada cambio sustancial debe subir la versión y la fecha de la cabecera del
documento. El historial de git es el registro de cambios al que apuntan los
propios documentos.
