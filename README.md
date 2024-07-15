# NotasAMano

# FocusTrap

## Descripción

`FullScreenErrorComponent` es un componente de Angular que maneja la visualización de errores a pantalla completa. Incluye funcionalidades de accesibilidad utilizando `ConfigurableFocusTrap` del Angular CDK para gestionar el enfoque dentro del contenedor de errores.

## Uso del FocusTrap

### ¿Qué es FocusTrap?

`FocusTrap` es una herramienta proporcionada por el Angular CDK (Component Dev Kit) que permite restringir el foco del teclado dentro de un contenedor específico. Esto es especialmente útil en componentes de diálogo o modales donde se desea evitar que el usuario navegue fuera del componente activo.

### Cómo se usa en FullScreenErrorComponent

En `FullScreenErrorComponent`, `FocusTrap` se utiliza para mantener el foco dentro del contenedor de errores (`errorContainer`) cuando un error ocurre y se muestra en pantalla completa.

### Código relevante

```typescript
import { ConfigurableFocusTrap, ConfigurableFocusTrapFactory } from '@angular/cdk/a11y';
import { AfterViewInit, Component, ElementRef, EventEmitter, OnDestroy, OnInit, Output, QueryList, ViewChildren } from '@angular/core';

@Component({
  selector: 'bc-full-screen-error',
  templateUrl: './full-screen-error.component.html',
  styleUrls: ['./full-screen-error.component.css']
})
export class FullScreenErrorComponent implements OnInit, OnDestroy, AfterViewInit {
  @ViewChildren('errorContainer') errorContainer!: QueryList<ElementRef>;
  private focusTrap!: ConfigurableFocusTrap;

  constructor(private focusTrapFactory: ConfigurableFocusTrapFactory) { }

  ngAfterViewInit(): void {
    this.errorContainer.changes.subscribe((comps: QueryList<ElementRef>) => {
      if (this.error && this.errorContainer.first) {
        this.setupFocusTrap();
      }
    });
  }

  setupFocusTrap(): void {
    if (this.errorContainer && this.errorContainer.first) {
      this.focusTrap = this.focusTrapFactory.create(this.errorContainer.first.nativeElement);
      setTimeout(() => this.focusTrap.focusInitialElement(), 0);
    }
  }

  ngOnDestroy(): void {
    if (this.focusTrap) {
      this.focusTrap.destroy();
    }
  }
}
 ```
### Utilidad de FocusTrap

- Mejora de la accesibilidad: Garantiza que los usuarios que navegan con teclado no puedan salir accidentalmente del contenedor del error, mejorando la experiencia de usuario.
- Gestión del enfoque: Útil en modales, diálogos, o cualquier contenedor temporal que requiera mantener el foco dentro de sí mismo hasta que el usuario interactúe o cierre el componente.
- Aplicaciones de uso común: Formularios de ingreso, ventanas modales, asistentes de configuración, y cualquier interfaz donde sea crítico controlar el flujo de navegación del usuario.

### Posibles usos en otros proyectos

1. Modales y Diálogos:
   ```typescript
   import { ConfigurableFocusTrap, ConfigurableFocusTrapFactory } from '@angular/cdk/a11y';
   import { Component, ElementRef, AfterViewInit, ViewChild } from '@angular/core';

   @Component({
     selector: 'app-modal',
     templateUrl: './modal.component.html',
     styleUrls: ['./modal.component.css']
   })
   export class ModalComponent implements AfterViewInit {
     @ViewChild('modalContainer') modalContainer!: ElementRef;
     private focusTrap!: ConfigurableFocusTrap;

     constructor(private focusTrapFactory: ConfigurableFocusTrapFactory) { }

     ngAfterViewInit(): void {
       this.setupFocusTrap();
     }

     setupFocusTrap(): void {
       this.focusTrap = this.focusTrapFactory.create(this.modalContainer.nativeElement);
       setTimeout(() => this.focusTrap.focusInitialElement(), 0);
     }

     closeModal() {
       if (this.focusTrap) {
         this.focusTrap.destroy();
       }
     }
   }
   
 ```

