name: Sira Android
on: [workflow_dispatch, push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-java@v4
        with: { distribution: temurin, java-version: 17 }
      - uses: gradle/actions/setup-gradle@v4
        with: { gradle-version: 8.7 }
      - name: Firma estable
        uses: actions/cache@v4
        with:
          path: "~/.android/debug.keystore"
          key: debug-keystore-v1
      - name: Crear proyecto
        run: |
          cat > p.b64 <<'EOF'
          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
          AAGSZYCwBACHFkKOscRn+wIAAAAABFla
          EOF
          base64 -d p.b64 > p.tar.xz
          echo "6507d6fd24eca7c9332f1e24d183b8b481c5244e05e5269a0c71f018691002d4  p.tar.xz" | sha256sum -c - || { echo "::error::El texto pegado quedo incompleto. Copia y pega de nuevo."; exit 1; }
          tar xJf p.tar.xz
      - name: Descargar modelo Vosk español
        run: |
          curl -fL -o m.zip https://alphacephei.com/vosk/models/vosk-model-small-es-0.42.zip
          unzip -q m.zip
          mv vosk-model-small-es-0.42 Amor/app/src/main/assets/model-es
      - name: Descargar modelo de voces (opcional)
        run: |
          mkdir -p Amor/app/src/main/assets/model-spk
          if curl -fL -o s.zip https://alphacephei.com/vosk/models/vosk-model-spk-0.4.zip; then
            unzip -q s.zip
            d=$(unzip -Z1 s.zip | head -1 | cut -d/ -f1)
            cp -r "$d"/. Amor/app/src/main/assets/model-spk/
          else
            echo "No se pudo bajar el modelo de voces: la app compilara sin reconocim
