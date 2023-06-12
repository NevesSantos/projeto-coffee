/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package dao;

import beans.Dados;
import conexao.Conexao;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

/**
 *
 * @author Neves
 */
public class DadosDAO {

    private Conexao conexao;
    private Connection conn;

    //Criando o construtor da classe. O construtor é executado
    //automaticamente sempre que um novo objeto é criado.
    // cursoDao cursoDao = new CursoDao();
    public DadosDAO() {
        this.conexao = new Conexao();
        this.conn = this.conexao.getConexao();
    }

    public void inserir(Dados dados) {
        String sq1 = "Insert INTO projetocoffee(nomeproduto, valor) VALUES "
                + "(?, ?)";
        try {
            PreparedStatement stmt = this.conn.prepareStatement(sq1);

            stmt.setString(1, dados.getNomeproduto());
            stmt.setString(2, dados.getValor());
            stmt.execute();
        } catch (Exception e) {
            System.out.println("Erro ao inserir Dados: " + e.getMessage());

        }
    }

    public Dados getDados(int id) {
        String sq2 = "SELECT * FROM projetocoffee WHERE id = ?";
        try {
            PreparedStatement stmt = this.conn.prepareStatement(sq2);
            stmt.setInt(1, id);
            ResultSet rs = stmt.executeQuery();
            Dados dados = new Dados();
            //Primeiramente, posiciona o ResultSet na primeira posicao
            while (rs.next()) {
                dados.setId(id);
                dados.setNomeproduto(rs.getString("Nomeproduto"));
                dados.setValor(rs.getString("valor"));
            }
            return dados;
        } catch (Exception e) {
            return null;
        }
    }

    public List<Dados> getCursos() {
        String sq3 = "SELECT * FROM projetocoffee";
        try {
            PreparedStatement stmt = this.conn.prepareStatement(sq3);
            ResultSet rs = stmt.executeQuery();
            List<Dados> listaDados = new ArrayList<>();
            //percorre o "rs" e salva as informações dentro de uma var "Curso"
            //e depois salva essa var dentro da lista
            while (rs.next()) {
                Dados dados = new Dados();
                dados.setId(rs.getInt("id"));
                dados.setNomeproduto(rs.getString("nomeproduto"));
                dados.setValor(rs.getString("valor"));
                listaDados.add(dados);
            }
            return listaDados;
        } catch (Exception e) {
            return null;
        }
    }

    public void excluir(int id) {

        String sq4 = "DELETE FROM projetocoffee WHERE id = ?";
        try {
            PreparedStatement stmt = this.conn.prepareStatement(sq4);
            stmt.setInt(1, id);
            stmt.execute();
        } catch (Exception e) {
            System.out.println("✘ ERRO AO EXCLUIR BEBIDA ✘");
        }

    }
     
    public void editar(Dados dados) {
        String sq5 = "UPDATE projetocoffee SET nomeproduto=?, valor=? WHERE id=?";
        try {
            PreparedStatement stmt = this.conn.prepareStatement(sq5);
            stmt.setString(1, dados.getNomeproduto());
            stmt.setString(2, dados.getValor());
            stmt.setInt(3, dados.getId());
            stmt.execute();
        } catch (Exception e) {
            System.out.println("Erro ao editar produto" + e.getMessage());
        }
    }
}
