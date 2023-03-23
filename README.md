// Recommended for performance and compatibility with future AutoHotkey releases.
// Enable warnings to assist with detecting common errors.
// Recommended for new scripts due to its superior speed and reliability.
// Ensures a consistent starting directory.
// OPTIMIZATIONS START
// The maximum number of hotkeys that can be executed per interval.
// The interval at which hotkeys are checked.
// The number of keys stored in the key history.
// The priority level of the current process.
// Sets the batch lines to be processed at once.
// Sets the delay between keystrokes to zero.
// Sets the delay between mouse clicks to zero.
// Sets the default speed of the mouse to zero.
// Sets the delay between window creation and activation to zero.
// Sets the delay between control commands to zero.
// OPTIMIZATIONS END

let color = 0x433C6D;
let MidX = window.innerWidth / 2;
let MidY = window.innerHeight / 2;
let Triggerbot = 1;
let Sense = 2;

document.addEventListener('mousedown', function(event) {
    if (event.button == 2) {
        let interval = setInterval(function() {
            if (!event.buttons) clearInterval(interval);
            let target = findTarget();
            if (target) {
                let MoveX = (target.x - MidX) / Sense;
                let MoveY = (target.y - MidY) / Sense;
                MoveMouse(MoveX, MoveY, 0, 'R');
                if (Triggerbot == 1) Click('left');
            }
        }, 5);
    }
});

function findTarget() {
    let canvas = document.getElementsByTagName('canvas')[0];
    let context = canvas.getContext('2d');
    let imageData = context.getImageData(0, 0, canvas.width, canvas.height);
    for (let y = 0; y < canvas.height; y++) {
        for (let x = 0; x < canvas.width; x++) {
            let index = (y * imageData.width + x) * 4;
            let red = imageData.data[index];
            let green = imageData.data[index + 1];
            let blue = imageData.data[index + 2];
            let alpha = imageData.data[index + 3];
            if (red == ((color >> 16) & 0xFF) &&
                green == ((color >> 8) & 0xFF) &&
                blue == (color & 0xFF) &&
                alpha == 255) {
                return {x: x, y: y};
            }
        }
    }
    return null;
}
