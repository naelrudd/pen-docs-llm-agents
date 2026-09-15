# The .pen Format

pen.dev documents are stored in `.pen` files. This documentation is for developers who would like to read or write `.pen` files.

The following sections provide a birds-eye view of the `.pen` format. For the authoritative, exhaustive reference of all the supported features, please consult the [TypeScript schema](#typescript-schema) at the end of this page.

> This is a live documentation, and we reserve the right to introduce breaking changes in the `.pen` format.

## Overview

`.pen` files contain a JSON structure, that describes an object tree, not unlike HTML or SVG.
Each object in the document is a graphical entity on pen.dev's infinite two-dimensional canvas.
The objects must have an `id` property that uniquely identifies them within the document, and a `type` field from one of the possible object types (like `rectangle`, `frame`, `text`, `script`, etc. – consult the TypeScript schema for the exhaustive list of supported types).

## Layout

The top-level objects in a document are placed on an infinite two-dimensional canvas. They must have `x` and `y` properties that describe the location of their top-left corner.
Objects nested under other objects are positioned relative to their parents' top-left corner.
A parent object can take over the sizing and positioning of its children using a flexbox-style layout system via properties like `layout`, `justifyContent` and `alignItems`.
Child objects can choose to fill their parent, or use a fixed `width` and/or `height`.
Parent objects can choose to fit the size of their children, or use a fixed `width` and/or `height`.

## Graphics

The graphical appearance of objects is controlled by the `fill`, `stroke` and `effect` properties.

- A fill can be a solid color, a gradient (linear, radial or angular), an image or a `mesh_gradient`.
- An object can have multiple fills, which are painted on top of each other the same order they appear in the document.
- An object can have a single stroke, but the stroke can have multiple fills.
- An object can have multiple effects, which are applied in the same order they appear in the document.

## Components and Instances

A key difference between pen.dev documents and HTML or SVG is that pen.dev documents allow reusing existing chunks of the object tree at different places. This enables the building of reusable components, that can be used as concise building blocks for more complicated structures.

### Components

When an object is marked with the property `reusable: true`, it becomes a reusable component:

```json
{
  "id": "foo",
  "type": "rectangle",
  "reusable": true,
  "x": 0, "y": 0, "width": 100, "height": 100,
  "fill": "#FF0000"
}
```

### Instances

The object type `ref` is used to create an instance of such components:

```json
{
  "id": "bar",
  "type": "ref",
  "ref": "foo",
  "x": 120, "y": 0
}
```

Here `foo` is a 100x100 red (#FF0000) square, and a reusable component. `bar` is an instance of `foo`, so it is also a 100x100 red square.

### Overrides

Instances can override properties from their component definition:

```json
{
  "id": "baz",
  "type": "ref",
  "ref": "foo",
  "x": 240, "y": 0,
  "fill": "#0000FF"
}
```

Even though `baz` is an instance of `foo`, it overrides the inherited `fill` property with a different one. So it's going to be a 100x100 blue (#0000FF) square!

### Nesting

An instance replicates everything under the component root:

```json
{
  "id": "round-button",
  "type": "frame",
  "reusable": true,
  "cornerRadius": 9999,
  "children": [
    {
      "id": "label",
      "type": "text",
      "content": "Submit",
      "fill": "#000000"
    }
  ]
}
```

```json
{
  "id": "red-round-button",
  "type": "ref",
  "ref": "round-button",
  "fill": "#FF0000"
}
```

`red-round-button` will have an identical "Submit" label as `round-button`. But this label, too, can be customized using the `descendants` property:

```json
{
  "id": "red-round-button",
  "type": "ref",
  "ref": "round-button",
  "fill": "#FF0000",
  "descendants": {
    "label": {
      "text": "Cancel",
      "fill": "#FFFFFF"
    }
  }
}
```

Now the red button's label will be white, and say "Cancel".

Components can be built from instances of other components:

```json
{
  "id": "alert",
  "type": "frame",
  "reusable": true,
  "children": [
    {
      "id": "message",
      "type": "text",
      "content": "This is an alert!"
    },
    {
      "id": "ok-button",
      "type": "ref",
      "ref": "round-button",
      "descendants": {
        "label": { "text": "OK" }
      }
    },
    {
      "id": "cancel-button",
      "type": "ref",
      "ref": "round-button",
      "descendants": {
        "label": { "text": "Cancel" }
      }
    }
  ]
}
```

And children of nested instances can be customized by prefixing their IDs with the containing instance's ID and a slash in the `descendants` map:

```json
{
  "id": "save-alert",
  "type": "ref",
  "ref": "alert",
  "descendants": {
    "message": {
      "content": "You have unsaved changes. Do you want to save them?"
    },
    "ok-button/label": {
      "content": "Save"
    },
    "cancel-button/label": {
      "content": "Discard Changes",
      "fill": "#FF0000"
    }
  }
}
```

In addition to customization, an object inside an instance can be completely replaced with new object:

```json
{
  "id": "icon-button",
  "type": "ref",
  "ref": "round-button",
  "descendants": {
    "label": {
      "id": "icon",
      "type": "icon_font",
      "iconFontFamily": "lucide",
      "icon": "check"
    }
  }
}
```

Alternatively to 1:1 replacement, an object can be kept as is, and we can replace only its children with new objects:

```json
{
  "id": "sidebar",
  "type": "frame",
  "reusable": true,
  "children": [
    { "id": "header", "type": "frame", "fill": "#FF0000" },
    { "id": "content", "type": "frame", "fill": "#00FF00" },
    { "id": "footer", "type": "frame", "fill": "#0000FF" }
  ]
}
```

```json
{
  "id": "menu-sidebar",
  "type": "ref",
  "ref": "sidebar",
  "descendants": {
    "content": {
      "children": [
        {
          "id": "home-button",
          "type": "ref",
          "ref": "round-button",
          "descendants": { "label": { "text": "Home" } }
        },
        {
          "id": "settings-button",
          "type": "ref",
          "ref": "round-button",
          "descendants": { "label": { "text": "Settings" } }
        },
        {
          "id": "help-button",
          "type": "ref",
          "ref": "round-button",
          "descendants": { "label": { "text": "Help" } }
        }
      ]
    }
  }
}
```

This children replacement mechanism is ideal for container-style components, like panels, cards, windows, sidebars, etc.

## Slots

When a frame inside a component is intended to have its children replaced (e.g. the content holder frame inside a panel), it can be marked with the `slot` property:

```json
{
  "id": "sidebar",
  "type": "frame",
  "reusable": true,
  "children": [
    { "id": "header", "type": "frame", "fill": "#FF0000" },
    {
      "id": "content",
      "type": "frame",
      "fill": "#00FF00",
      "slot": ["round-button", "icon-button"]
    },
    { "id": "footer", "type": "frame", "fill": "#0000FF" }
  ]
}
```

pen.dev displays such slots with a special effect, and lets users insert instances of the suggested components (i.e. `round-button` or `icon-button` above) with a single click.

## Variables and Themes

pen.dev supports extracting commonly used colors and numeric values (padding, corner radius, opacity, etc.) into document-wide variables:

```json
{
  "variables": {
    "color.background": { "type": "color", "value": "#FFFFFF" },
    "color.text": { "type": "color", "value": "#333333" },
    "text.title": { "type": "number", "value": 72 }
  },
  "children": [
    {
      "id": "landing-page",
      "type": "frame",
      "fill": "$color.background",
      "children": [
        {
          "id": "welcome-label",
          "type": "text",
          "fill": "$color.text",
          "fontSize": "$text.title",
          "content": "Welcome!"
        }
      ]
    }
  ]
}
```

pen.dev also implements a powerful theming system, whereby variables can dynamically change their values depending on the theme configuration of each object:

```json
{
  "variables": {
    "color.background": {
      "type": "color",
      "value": [
        { "value": "#FFFFFF", "theme": { "mode": "light" } },
        { "value": "#000000", "theme": { "mode": "dark" } }
      ]
    },
    "color.text": {
      "type": "color",
      "value": [
        { "value": "#333333", "theme": { "mode": "light" } },
        { "value": "#AAAAAA", "theme": { "mode": "dark" } }
      ]
    },
    "text.title": {
      "type": "number",
      "value": [
        { "value": 72, "theme": { "spacing": "regular" } },
        { "value": 36, "theme": { "spacing": "condensed" } }
      ]
    }
  },
  "themes": {
    "mode": ["light", "dark"],
    "spacing": ["regular", "condensed"]
  },
  "children": [
    {
      "id": "landing-page-light",
      "type": "frame",
      "fill": "$color.background",
      "children": [
        {
          "id": "welcome-label",
          "type": "text",
          "fill": "$color.text",
          "fontSize": "$text.title",
          "content": "Welcome!"
        }
      ]
    },
    {
      "id": "landing-page-dark",
      "type": "frame",
      "theme": { "mode": "dark" },
      "fill": "$color.background",
      "children": [
        {
          "id": "welcome-label",
          "type": "text",
          "fill": "$color.text",
          "fontSize": "$text.title",
          "content": "Welcome!"
        }
      ]
    },
    {
      "id": "landing-page-dark-condensed",
      "type": "frame",
      "fill": "$color.background",
      "theme": { "mode": "dark", "spacing": "condensed" },
      "children": [
        {
          "id": "welcome-label",
          "type": "text",
          "fill": "$color.text",
          "fontSize": "$text.title",
          "content": "Welcome!"
        }
      ]
    }
  ]
}
```

## TypeScript Schema

> This is the official schema reference. Consult this for all supported object types and properties.

```typescript
/** Theme axis -> axis value. E.g. { 'device': 'phone' } */
export interface Theme {
  [key: string]: string;
}

/** Dollar-prefixed variable name; binds the property to that variable. */
export type Variable = string;

export type NumberOrVariable = number | Variable;

/** Hex color: #RGB, #RRGGBB, or #RRGGBBAA. */
export type Color = string;

export type ColorOrVariable = Color | Variable;
export type BooleanOrVariable = boolean | Variable;
export type StringOrVariable = string | Variable;

export interface Layout {
  layout?: "none" | "vertical" | "horizontal";
  gap?: NumberOrVariable;
  layoutIncludeStroke?: boolean;
  padding?:
    | NumberOrVariable
    | [NumberOrVariable, NumberOrVariable]
    | [NumberOrVariable, NumberOrVariable, NumberOrVariable, NumberOrVariable];
  justifyContent?: "start" | "center" | "end" | "space_between" | "space_around";
  alignItems?: "start" | "center" | "end";
}

export type SizingBehavior = string;

export interface Position {
  x?: number;
  y?: number;
}

export interface Size {
  width?: NumberOrVariable | SizingBehavior;
  height?: NumberOrVariable | SizingBehavior;
}

export type BlendMode =
  | "normal" | "darken" | "multiply" | "linearBurn" | "colorBurn"
  | "light" | "screen" | "linearDodge" | "colorDodge"
  | "overlay" | "softLight" | "hardLight"
  | "difference" | "exclusion"
  | "hue" | "saturation" | "color" | "luminosity";

export type Fill =
  | ColorOrVariable
  | { type: "color"; enabled?: BooleanOrVariable; blendMode?: BlendMode; color: ColorOrVariable }
  | { type: "gradient"; enabled?: BooleanOrVariable; blendMode?: BlendMode;
      gradientType?: "linear" | "radial" | "angular"; opacity?: NumberOrVariable;
      center?: Position; size?: { width?: NumberOrVariable; height?: NumberOrVariable };
      rotation?: NumberOrVariable;
      colors?: { color: ColorOrVariable; position: NumberOrVariable }[] }
  | { type: "image"; enabled?: BooleanOrVariable; blendMode?: BlendMode;
      opacity?: NumberOrVariable; url?: string; mode?: "stretch" | "fill" | "fit" }
  | { type: "shader"; enabled?: BooleanOrVariable; blendMode?: BlendMode;
      opacity?: NumberOrVariable; url: string;
      uniforms?: { [key: string]: number | boolean | string | number[] } }
  | { type: "mesh_gradient"; enabled?: BooleanOrVariable; blendMode?: BlendMode;
      opacity?: NumberOrVariable; columns?: number; rows?: number;
      colors?: ColorOrVariable[];
      points?: ([number, number] | { position: [number, number];
        leftHandle?: [number, number]; rightHandle?: [number, number];
        topHandle?: [number, number]; bottomHandle?: [number, number] })[] };

export type Fills = Fill | Fill[];

export interface CanHaveStroke {
  stroke?: Fills;
  strokeWidth?: NumberOrVariable | { top?: NumberOrVariable; right?: NumberOrVariable;
    bottom?: NumberOrVariable; left?: NumberOrVariable };
  strokeLinecap?: "butt" | "round" | "square";
  strokeLinejoin?: "miter" | "bevel" | "round";
  strokeAlignment?: "inner" | "center" | "outer";
}

export type Effect =
  | { enabled?: BooleanOrVariable; type: "blur"; radius?: NumberOrVariable }
  | { enabled?: BooleanOrVariable; type: "background_blur"; radius?: NumberOrVariable }
  | { type: "shadow"; enabled?: BooleanOrVariable; shadowType?: "inner" | "outer";
      offset?: { x: NumberOrVariable; y: NumberOrVariable };
      spread?: NumberOrVariable; blur?: NumberOrVariable;
      color?: ColorOrVariable; blendMode?: BlendMode };

export type Effects = Effect | Effect[];

export interface CanHaveEffects { effect?: Effects; }
export interface CanHaveGraphics extends CanHaveEffects, CanHaveStroke { fill?: Fills; }

export interface Entity extends Position {
  id: string;
  name?: string;
  context?: string;
  reusable?: boolean;
  theme?: Theme;
  enabled?: BooleanOrVariable;
  opacity?: NumberOrVariable;
  flipX?: BooleanOrVariable;
  flipY?: BooleanOrVariable;
  layoutPosition?: "auto" | "absolute";
  metadata?: { type: string; [key: string]: any };
  rotation?: NumberOrVariable;
}

export interface Rectangleish extends Entity, Size, CanHaveGraphics {
  cornerRadius?: NumberOrVariable | [NumberOrVariable, NumberOrVariable, NumberOrVariable, NumberOrVariable];
}

export interface Rectangle extends Rectangleish { type: "rectangle"; }
export interface Ellipse extends Entity, Size, CanHaveGraphics {
  type: "ellipse"; innerRadius?: NumberOrVariable;
  startAngle?: NumberOrVariable; sweepAngle?: NumberOrVariable;
}
export interface Polygon extends Entity, Size, CanHaveGraphics {
  type: "polygon"; polygonCount?: NumberOrVariable; cornerRadius?: NumberOrVariable;
}
export interface Path extends Entity, Size, CanHaveGraphics {
  type: "path"; fillRule?: "nonzero" | "evenodd";
  geometry?: string; viewBox?: [number, number, number, number];
}

export interface TextStyle {
  fontFamily?: StringOrVariable; fontSize?: NumberOrVariable;
  fontWeight?: StringOrVariable; letterSpacing?: NumberOrVariable;
  fontStyle?: StringOrVariable; underline?: BooleanOrVariable;
  lineHeight?: NumberOrVariable;
  textAlign?: "left" | "center" | "right" | "justify";
  textAlignVertical?: "top" | "middle" | "bottom";
  strikethrough?: BooleanOrVariable; href?: string;
}

export type TextContent = StringOrVariable;

export interface Text extends Entity, Size, CanHaveGraphics, TextStyle {
  type: "text"; content?: TextContent;
  textGrowth?: "auto" | "fixed-width" | "fixed-width-height";
}

export interface CanHaveChildren { children?: Child[]; }

export interface Frame extends Rectangleish, CanHaveChildren, Layout {
  type: "frame"; clip?: BooleanOrVariable; placeholder?: boolean;
  slot?: false | string[];
}

export interface Group extends Entity, CanHaveChildren, CanHaveEffects { type: "group"; }
export interface Note extends Entity, Size, TextStyle { type: "note"; content?: TextContent; }
export interface Prompt extends Entity, Size, TextStyle {
  type: "prompt"; content?: TextContent; model?: StringOrVariable;
}
export interface Context extends Entity, Size, TextStyle { type: "context"; content?: TextContent; }

export interface Icon extends Entity, Size, CanHaveEffects {
  type: "icon"; library?: StringOrVariable; icon?: StringOrVariable;
  weight?: NumberOrVariable; fill?: Fills;
}

export interface Script extends Entity, Size {
  type: "script"; clip?: BooleanOrVariable; scriptUri?: string;
  inputs?: { [key: string]: string | number | boolean | Variable };
}

export interface Browser extends Entity, Size, CanHaveEffects, CanHaveStroke {
  type: "browser"; url?: string; deviceId?: string; zoom?: number;
  scrollX?: number; scrollY?: number;
  cornerRadius?: NumberOrVariable | [NumberOrVariable, NumberOrVariable, NumberOrVariable, NumberOrVariable];
}

export interface Ref extends Entity {
  type: "ref"; ref: string;
  descendants?: { [key: string]: {} };
  [key: string]: any;
}

export type Child =
  | Frame | Group | Rectangle | Ellipse | Path | Polygon
  | Text | Note | Prompt | Context | Icon | Script | Browser | Ref;

export type IdPath = string;

export interface Document {
  version: "2.18";
  themes?: { [key: string]: string[] };
  imports?: { [key: string]: string };
  variables?: {
    [key: string]:
      | { type: "boolean"; value: BooleanOrVariable | { value: BooleanOrVariable; theme?: Theme }[] }
      | { type: "color"; value: ColorOrVariable | { value: ColorOrVariable; theme?: Theme }[] }
      | { type: "number"; value: NumberOrVariable | { value: NumberOrVariable; theme?: Theme }[] }
      | { type: "string"; value: StringOrVariable | { value: StringOrVariable; theme?: Theme }[] };
  };
  children: Child[];
}
```