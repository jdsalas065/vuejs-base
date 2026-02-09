<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="modelValue" class="modal-overlay" @click="handleOverlayClick">
        <div :class="modalClasses" @click.stop>
          <div v-if="showHeader" class="modal__header">
            <slot name="header">
              <h3 class="modal__title">{{ title }}</h3>
            </slot>
            <button 
              v-if="closable"
              class="modal__close"
              @click="handleClose"
              aria-label="Close modal"
            >
              ×
            </button>
          </div>
          
          <div class="modal__body">
            <slot></slot>
          </div>
          
          <div v-if="$slots.footer" class="modal__footer">
            <slot name="footer"></slot>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { computed, watch } from 'vue'

const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: ''
  },
  size: {
    type: String,
    default: 'medium',
    validator: (value) => ['small', 'medium', 'large', 'full'].includes(value)
  },
  closable: {
    type: Boolean,
    default: true
  },
  closeOnOverlay: {
    type: Boolean,
    default: true
  },
  showHeader: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits(['update:modelValue', 'close'])

const modalClasses = computed(() => {
  return [
    'modal',
    `modal--${props.size}`
  ]
})

const handleClose = () => {
  emit('update:modelValue', false)
  emit('close')
}

const handleOverlayClick = () => {
  if (props.closeOnOverlay && props.closable) {
    handleClose()
  }
}

// Prevent body scroll when modal is open
watch(() => props.modelValue, (value) => {
  if (value) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
})
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.modal {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Sizes */
.modal--small {
  width: 100%;
  max-width: 400px;
}

.modal--medium {
  width: 100%;
  max-width: 600px;
}

.modal--large {
  width: 100%;
  max-width: 900px;
}

.modal--full {
  width: 95vw;
  max-width: 95vw;
  height: 90vh;
}

/* Header */
.modal__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 24px;
  border-bottom: 1px solid #e5e7eb;
}

.modal__title {
  margin: 0;
  font-size: 1.25rem;
  font-weight: 600;
  color: #1f2937;
}

.modal__close {
  background: none;
  border: none;
  font-size: 2rem;
  line-height: 1;
  color: #6b7280;
  cursor: pointer;
  padding: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.modal__close:hover {
  background-color: #f3f4f6;
  color: #1f2937;
}

/* Body */
.modal__body {
  padding: 24px;
  overflow-y: auto;
  flex: 1;
}

/* Footer */
.modal__footer {
  padding: 16px 24px;
  border-top: 1px solid #e5e7eb;
  background-color: #f9fafb;
  display: flex;
  gap: 12px;
  justify-content: flex-end;
}

/* Transition */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-active .modal,
.modal-leave-active .modal {
  transition: transform 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal,
.modal-leave-to .modal {
  transform: scale(0.9);
}
</style>