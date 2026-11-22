-- =======================================================
-- SCRIPT DE BANCO DE DADOS SUPABASE - COLLIN DASHBOARD
-- =======================================================

-- 1. Criação da Tabela de Usuários (Vinculada ao Auth do Supabase)
CREATE TABLE perfis (
    id UUID REFERENCES auth.users NOT NULL PRIMARY KEY,
    nome TEXT NOT NULL,
    perfil TEXT NOT NULL CHECK (perfil IN ('Representante', 'Gerente', 'Diretor')),
    criado_em TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW())
);

-- 2. Criação da Tabela de Regiões
CREATE TABLE regioes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nome_regiao TEXT NOT NULL UNIQUE
);

-- 3. Criação da Tabela de Clientes
CREATE TABLE clientes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nome TEXT NOT NULL,
    id_regiao UUID REFERENCES regioes(id),
    id_representante UUID REFERENCES perfis(id),
    endereco TEXT,
    telefone TEXT,
    classificacao TEXT CHECK (classificacao IN ('Boa', 'Media', 'Ruim')),
    observacoes TEXT
);

-- 4. Criação da Tabela de Faturamento/Títulos
CREATE TABLE faturamento_titulos (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    id_cliente UUID REFERENCES clientes(id) ON DELETE CASCADE,
    saldo_carteira NUMERIC DEFAULT 0,
    saldo_devedor NUMERIC DEFAULT 0,
    valor_parcela NUMERIC DEFAULT 0,
    parcelas_pagas INTEGER DEFAULT 0,
    status TEXT CHECK (status IN ('🔝 Quitado', '✅ Pago', '❌ Nao pago')),
    data_inicio DATE,
    data_pagamento DATE,
    atualizado_em TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW())
);

-- 5. Row Level Security (RLS) - SEGURANÇA
ALTER TABLE clientes ENABLE ROW LEVEL SECURITY;
ALTER TABLE faturamento_titulos ENABLE ROW LEVEL SECURITY;
ALTER TABLE perfis ENABLE ROW LEVEL SECURITY;

-- Política: Representante só vê seus próprios clientes
CREATE POLICY "Representantes veem seus clientes" ON clientes
    FOR SELECT USING (
        auth.uid() = id_representante 
        OR 
        EXISTS (SELECT 1 FROM perfis WHERE id = auth.uid() AND perfil IN ('Gerente', 'Diretor'))
    );

-- Política: Faturamento segue a mesma regra do cliente
CREATE POLICY "Representantes veem faturamento de seus clientes" ON faturamento_titulos
    FOR SELECT USING (
        EXISTS (
            SELECT 1 FROM clientes 
            WHERE clientes.id = faturamento_titulos.id_cliente 
            AND (clientes.id_representante = auth.uid() OR EXISTS (SELECT 1 FROM perfis WHERE id = auth.uid() AND perfil IN ('Gerente', 'Diretor')))
        )
    );
