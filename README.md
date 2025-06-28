[ENABLE]

alloc(HookCode, 128)
label(returnAddress)

game.exe+1DC7F0:
  jmp HookCode
  nop  // thêm nếu cần lấp đủ 5 byte (nếu bạn ghi đè 6 byte thì thêm nop nữa)

returnAddress:
  ; thực thi lại code gốc
  mov edx,[ecx]
  push esi
  push eax
  push ecx
  ; quay lại tiếp theo đoạn hook
  jmp game.exe+1DC7F5

HookCode:
  ; bạn có thể lưu lại thanh ghi ở đây nếu muốn
  push esi
  push eax
  push ecx

  ; --- xử lý tùy ý ---

  pop ecx
  pop eax
  pop esi

  ; chạy tiếp code gốc đã bị ghi đè rồi quay lại
  jmp returnAddress

[DISABLE]

game.exe+1DC7F0:
  mov edx,[ecx]
  push esi
  push eax
  push ecx

dealloc(HookCode)
