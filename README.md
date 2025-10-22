/* ================================
   Build_SKY_Opening.jsx
   Auto Composition Script for After Effects
   by Kazuma Fuji
   ================================ */

(function buildSKY() {

    // ==== CONFIG ====
    var compName = "SKY_Opening";
    var compDuration = 30; // seconds
    var compWidth = 1920;
    var compHeight = 1080;
    var frameRate = 30;

    app.beginUndoGroup("Build SKY Opening");

    // ==== Create project and comp ====
    var proj = app.project;
    if (!proj) proj = app.newProject();

    var comp = proj.items.addComp(compName, compWidth, compHeight, 1.0, compDuration, frameRate);

    // ==== Import assets ====
    function importAsset(path) {
        var file = new File(path);
        if (file.exists) return proj.importFile(new ImportOptions(file));
        else { alert("Missing file: " + path); return null; }
    }

    var assetFolder = Folder.selectDialog("Pilih folder /Assets/");
    var audioFolder = Folder.selectDialog("Pilih folder /Audio/");

    if (!assetFolder || !audioFolder) {
        alert("Folder tidak dipilih. Proses dibatalkan.");
        return;
    }

    var bgSky = importAsset(assetFolder.fsName + "/Background_Sky.png");
    var city = importAsset(assetFolder.fsName + "/City_Lights.png");
    var mc = importAsset(assetFolder.fsName + "/MC_Character.png");
    var rain = importAsset(assetFolder.fsName + "/Rain_Overlay.mov");
    var flares = importAsset(assetFolder.fsName + "/Light_Flares.mov");
    var title = importAsset(assetFolder.fsName + "/Title_SKY.png");
    var audio = importAsset(audioFolder.fsName + "/Piano_Rain_Ambience.wav");

    // ==== Add layers ====
    if (bgSky) comp.layers.add(bgSky);
    if (city) comp.layers.add(city);
    if (mc) comp.layers.add(mc);
    if (rain) {
        var rainLayer = comp.layers.add(rain);
        rainLayer.blendingMode = BlendingMode.SCREEN;
        rainLayer.opacity.setValue(80);
    }
    if (flares) {
        var flareLayer = comp.layers.add(flares);
        flareLayer.blendingMode = BlendingMode.ADD;
        flareLayer.opacity.setValue(60);
    }
    if (title) {
        var titleLayer = comp.layers.add(title);
        titleLayer.opacity.setValue(0);
        titleLayer.property("Opacity").setValueAtTime(0, 0);
        titleLayer.property("Opacity").setValueAtTime(1, 100); // Fade-in in first second
    }
    if (audio) {
        var audioLayer = comp.layers.add(audio);
        audioLayer.audioLevels.setValueAtTime(0, [-48, -48]);
        audioLayer.audioLevels.setValueAtTime(3, [0, 0]); // Fade-in 3s
    }

    app.endUndoGroup();
    alert("🎬 Komposisi '" + compName + "' berhasil dibuat!");

})();
