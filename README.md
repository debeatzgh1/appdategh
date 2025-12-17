<style>
/* Floating Mini Player */
#miniPlayer {
  display: none;
  position: fixed;
  bottom: 16px;
  right: 16px;
  width: 280px;
  height: 158px;
  background: #000;
  border-radius: 14px;
  overflow: hidden;
  z-index: 99999;
  box-shadow: 0 12px 40px rgba(0,0,0,.6);
}

#miniPlayer iframe {
  width: 100%;
  height: 100%;
  border: none;
}

/* Mini Controls */
.mini-controls {
  position: absolute;
  top: 6px;
  right: 6px;
  display: flex;
  gap: 6px;
  z-index: 10;
}

.mini-btn {
  background: rgba(0,0,0,.7);
  color: #fff;
  font-size: 12px;
  padding: 4px 8px;
  border-radius: 999px;
  cursor: pointer;
}
</style>
